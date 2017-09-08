---
title: "Configuração no núcleo do ASP.NET"
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
ms.openlocfilehash: dae7ac6e377d2c17bc8f86e5b6da98107366cc73
ms.sourcegitcommit: 418e6aa4ab79474ecc4d0a6af573a3759b113fe4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2017
---
<a name=fundamentals-configuration></a>

  # <a name="configuration-in-aspnet-core"></a><span data-ttu-id="06dd3-104">Configuração no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="06dd3-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="06dd3-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [marca Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](http://ardalis.com), e [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="06dd3-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](http://ardalis.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="06dd3-106">A API de configuração fornece uma maneira de configurar um aplicativo baseado em uma lista de pares nome-valor.</span><span class="sxs-lookup"><span data-stu-id="06dd3-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="06dd3-107">Configuração é lida em tempo de execução de várias fontes.</span><span class="sxs-lookup"><span data-stu-id="06dd3-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="06dd3-108">Os pares nome-valor podem ser agrupados em uma hierarquia de vários níveis.</span><span class="sxs-lookup"><span data-stu-id="06dd3-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="06dd3-109">Há provedores de configuração para:</span><span class="sxs-lookup"><span data-stu-id="06dd3-109">There are configuration providers for:</span></span>

* <span data-ttu-id="06dd3-110">Formatos de arquivo (INI, JSON e XML)</span><span class="sxs-lookup"><span data-stu-id="06dd3-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="06dd3-111">Argumentos de linha de comando</span><span class="sxs-lookup"><span data-stu-id="06dd3-111">Command-line arguments</span></span>
* <span data-ttu-id="06dd3-112">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="06dd3-112">Environment variables</span></span>
* <span data-ttu-id="06dd3-113">Objetos do .NET na memória</span><span class="sxs-lookup"><span data-stu-id="06dd3-113">In-memory .NET objects</span></span>
* <span data-ttu-id="06dd3-114">Um repositório de usuário criptografado</span><span class="sxs-lookup"><span data-stu-id="06dd3-114">An encrypted user store</span></span>
* [<span data-ttu-id="06dd3-115">Cofre de chaves do Azure</span><span class="sxs-lookup"><span data-stu-id="06dd3-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="06dd3-116">Provedores personalizados, que você instala ou criar</span><span class="sxs-lookup"><span data-stu-id="06dd3-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="06dd3-117">Cada valor de configuração é mapeado para uma chave de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="06dd3-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="06dd3-118">Não há suporte de ligação interna ao desserializar as configurações em um personalizado [POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object) objeto (uma classe .NET simple com propriedades).</span><span class="sxs-lookup"><span data-stu-id="06dd3-118">There's built-in binding support to deserialize settings into a custom [POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

[<span data-ttu-id="06dd3-119">Exibir ou baixar o código de exemplo</span><span class="sxs-lookup"><span data-stu-id="06dd3-119">View or download sample code</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a><span data-ttu-id="06dd3-120">Configuração simples</span><span class="sxs-lookup"><span data-stu-id="06dd3-120">Simple configuration</span></span>

<span data-ttu-id="06dd3-121">O aplicativo de console a seguir usa o provedor de configuração JSON:</span><span class="sxs-lookup"><span data-stu-id="06dd3-121">The following console app uses the JSON configuration provider:</span></span>

<span data-ttu-id="06dd3-122">[!code-csharp[Main](configuration/sample/src/ConfigJson/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-122">[!code-csharp[Main](configuration/sample/src/ConfigJson/Program.cs)]</span></span>

<span data-ttu-id="06dd3-123">O aplicativo lê e exibe as seguintes configurações:</span><span class="sxs-lookup"><span data-stu-id="06dd3-123">The app reads and displays the following configuration settings:</span></span>

<span data-ttu-id="06dd3-124">[!code-json[Main](configuration/sample/src/ConfigJson/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-124">[!code-json[Main](configuration/sample/src/ConfigJson/appsettings.json)]</span></span>

<span data-ttu-id="06dd3-125">Configuração consiste em uma lista hierárquica de pares nome-valor no qual os nós são separados por dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="06dd3-125">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="06dd3-126">Para recuperar um valor, acessar o `Configuration` indexador com a chave do item correspondente:</span><span class="sxs-lookup"><span data-stu-id="06dd3-126">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="06dd3-127">Para trabalhar com matrizes em fontes de configuração formatados em JSON, use um índice de matriz como parte da cadeia de caracteres separada por dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="06dd3-127">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="06dd3-128">O exemplo a seguir obtém o nome do primeiro item na anterior `wizards` matriz:</span><span class="sxs-lookup"><span data-stu-id="06dd3-128">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="06dd3-129">Pares de nome-valor gravados interna em `Configuration` provedores são **não** persistentes, no entanto, você pode criar um provedor personalizado que salva valores.</span><span class="sxs-lookup"><span data-stu-id="06dd3-129">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="06dd3-130">Consulte [provedor de configuração personalizado](xref:fundamentals/configuration#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="06dd3-130">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="06dd3-131">O exemplo anterior usa o indexador de configuração para ler valores.</span><span class="sxs-lookup"><span data-stu-id="06dd3-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="06dd3-132">A configuração de acesso fora de `Startup`, use o [padrão de opções](xref:fundamentals/configuration#options-config-objects).</span><span class="sxs-lookup"><span data-stu-id="06dd3-132">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="06dd3-133">O *padrão de opções* mostrado mais adiante neste artigo.</span><span class="sxs-lookup"><span data-stu-id="06dd3-133">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="06dd3-134">É comum ter configurações diferentes para ambientes diferentes, por exemplo, desenvolvimento, teste e produção.</span><span class="sxs-lookup"><span data-stu-id="06dd3-134">It's typical to have different configuration settings for different environments, for example, development, test and production.</span></span> <span data-ttu-id="06dd3-135">O seguinte código adiciona dois provedores de configuração de três fontes:</span><span class="sxs-lookup"><span data-stu-id="06dd3-135">The following highlighted code adds two configuration providers to three sources:</span></span>

1. <span data-ttu-id="06dd3-136">Provedor JSON, lendo *appSettings. JSON*</span><span class="sxs-lookup"><span data-stu-id="06dd3-136">JSON provider, reading *appsettings.json*</span></span>
2. <span data-ttu-id="06dd3-137">Provedor JSON, lendo *appsettings.\< EnvironmentName >. JSON*</span><span class="sxs-lookup"><span data-stu-id="06dd3-137">JSON provider, reading *appsettings.\<EnvironmentName>.json*</span></span>
3. <span data-ttu-id="06dd3-138">Provedor de variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="06dd3-138">Environment variables provider</span></span>

<span data-ttu-id="06dd3-139">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Startup.cs?name=snippet2&highlight=7-9)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-139">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Startup.cs?name=snippet2&highlight=7-9)]</span></span>

<span data-ttu-id="06dd3-140">Consulte [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) para obter uma explicação dos parâmetros.</span><span class="sxs-lookup"><span data-stu-id="06dd3-140">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="06dd3-141">`reloadOnChange`só é suportado no ASP.NET Core 1.1 e superior.</span><span class="sxs-lookup"><span data-stu-id="06dd3-141">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="06dd3-142">Fontes de configuração são lidos na ordem em que eles sejam especificados.</span><span class="sxs-lookup"><span data-stu-id="06dd3-142">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="06dd3-143">No código acima, as variáveis de ambiente são lidos pela última.</span><span class="sxs-lookup"><span data-stu-id="06dd3-143">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="06dd3-144">Quaisquer valores de configuração definido através do ambiente substituiria aquelas definidas em dois provedores anteriores.</span><span class="sxs-lookup"><span data-stu-id="06dd3-144">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="06dd3-145">O ambiente normalmente é definido como um dos `Development`, `Staging`, ou `Production`.</span><span class="sxs-lookup"><span data-stu-id="06dd3-145">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="06dd3-146">Consulte [trabalhando com vários ambientes](environments.md) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="06dd3-146">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="06dd3-147">Considerações de configuração:</span><span class="sxs-lookup"><span data-stu-id="06dd3-147">Configuration considerations:</span></span>

* <span data-ttu-id="06dd3-148">`IOptionsSnapshot`pode recarregar os dados de configuração quando ele é alterado.</span><span class="sxs-lookup"><span data-stu-id="06dd3-148">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="06dd3-149">Use `IOptionsSnapshot` se você precisar recarregar os dados de configuração.</span><span class="sxs-lookup"><span data-stu-id="06dd3-149">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="06dd3-150">Consulte [IOptionsSnapshot](#ioptionssnapshot) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="06dd3-150">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="06dd3-151">Chaves de configuração diferenciam maiusculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="06dd3-151">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="06dd3-152">Uma prática recomendada é especificar variáveis de ambiente por último, para que o ambiente local pode substituir qualquer coisa definidas nos arquivos de configuração implantada.</span><span class="sxs-lookup"><span data-stu-id="06dd3-152">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="06dd3-153">**Nunca** armazenar senhas ou outros dados confidenciais no código do provedor de configuração ou nos arquivos de configuração de texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="06dd3-153">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="06dd3-154">Não usa segredos de produção no desenvolvimento ou ambientes de teste.</span><span class="sxs-lookup"><span data-stu-id="06dd3-154">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="06dd3-155">Em vez disso, especifique segredos fora da árvore de projeto para que eles não puderem ser acidentalmente confirmados para o repositório.</span><span class="sxs-lookup"><span data-stu-id="06dd3-155">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="06dd3-156">Saiba mais sobre [trabalhando com vários ambientes](environments.md) e gerenciando [armazenamento seguro de segredos do aplicativo durante o desenvolvimento](../security/app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="06dd3-156">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="06dd3-157">Se `:` não pode ser usado em variáveis de ambiente em seu sistema, substitua `:` com `__` (sublinhado duplo).</span><span class="sxs-lookup"><span data-stu-id="06dd3-157">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="06dd3-158">Usando opções e objetos de configuração</span><span class="sxs-lookup"><span data-stu-id="06dd3-158">Using Options and configuration objects</span></span>

<span data-ttu-id="06dd3-159">O padrão de opções usa as classes de opções personalizada para representar um grupo de configurações relacionadas.</span><span class="sxs-lookup"><span data-stu-id="06dd3-159">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="06dd3-160">É recomendável que você crie classes separadas para cada recurso dentro de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="06dd3-160">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="06dd3-161">Execute as classes separadas:</span><span class="sxs-lookup"><span data-stu-id="06dd3-161">Decoupled classes follow:</span></span>

* <span data-ttu-id="06dd3-162">O [princípio de diferenciação de Interface (ISP)](http://deviq.com/interface-segregation-principle/) : Classes dependem apenas as definições de configuração que eles usam.</span><span class="sxs-lookup"><span data-stu-id="06dd3-162">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="06dd3-163">[Separação de preocupações](http://deviq.com/separation-of-concerns/) : configurações para diferentes partes do seu aplicativo não são dependentes ou combinado com uma da outra.</span><span class="sxs-lookup"><span data-stu-id="06dd3-163">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="06dd3-164">A classe de opções deve ser não-abstrato com um construtor público sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="06dd3-164">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="06dd3-165">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="06dd3-165">For example:</span></span>

<span data-ttu-id="06dd3-166">[!code-csharp[Main](configuration/sample/src/UsingOptions/Models/MyOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-166">[!code-csharp[Main](configuration/sample/src/UsingOptions/Models/MyOptions.cs)]</span></span>

<a name=options-example></a>

<span data-ttu-id="06dd3-167">No código a seguir, o provedor de configuração JSON está habilitado.</span><span class="sxs-lookup"><span data-stu-id="06dd3-167">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="06dd3-168">O `MyOptions` classe é adicionada ao contêiner de serviço e associada à configuração.</span><span class="sxs-lookup"><span data-stu-id="06dd3-168">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

<span data-ttu-id="06dd3-169">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-22)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-169">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-22)]</span></span>

<span data-ttu-id="06dd3-170">O seguinte [controlador](../mvc/controllers/index.md) usa [construtor injeção de dependência](xref:fundamentals/dependency-injection#what-is-dependency-injection) na [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) para acessar as configurações:</span><span class="sxs-lookup"><span data-stu-id="06dd3-170">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

<span data-ttu-id="06dd3-171">[!code-csharp[Main](configuration/sample/src/UsingOptions/Controllers/HomeController.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-171">[!code-csharp[Main](configuration/sample/src/UsingOptions/Controllers/HomeController.cs?name=snippet1)]</span></span>

<span data-ttu-id="06dd3-172">Com o seguinte *appSettings. JSON* arquivo:</span><span class="sxs-lookup"><span data-stu-id="06dd3-172">With the following *appsettings.json* file:</span></span>

<span data-ttu-id="06dd3-173">[!code-json[Main](configuration/sample/src/UsingOptions/appsettings1.json)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-173">[!code-json[Main](configuration/sample/src/UsingOptions/appsettings1.json)]</span></span>

<span data-ttu-id="06dd3-174">O `HomeController.Index` método retornará `option1 = value1_from_json, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="06dd3-174">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="06dd3-175">Aplicativos típicos não associar toda a configuração para um arquivo de opções de único.</span><span class="sxs-lookup"><span data-stu-id="06dd3-175">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="06dd3-176">Posteriormente, mostrarei como usar `GetSection` para vincular a uma seção.</span><span class="sxs-lookup"><span data-stu-id="06dd3-176">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="06dd3-177">No código a seguir, um segundo `IConfigureOptions<TOptions>` serviço é adicionado ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="06dd3-177">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="06dd3-178">Ele usa um delegado para configurar a associação com `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="06dd3-178">It uses a delegate to configure the binding with `MyOptions`.</span></span>

<span data-ttu-id="06dd3-179">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-179">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]</span></span>

<span data-ttu-id="06dd3-180">Você pode adicionar vários provedores de configuração.</span><span class="sxs-lookup"><span data-stu-id="06dd3-180">You can add multiple configuration providers.</span></span> <span data-ttu-id="06dd3-181">Provedores de configuração estão disponíveis em pacotes do NuGet.</span><span class="sxs-lookup"><span data-stu-id="06dd3-181">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="06dd3-182">Elas são aplicadas na ordem que eles são registrados.</span><span class="sxs-lookup"><span data-stu-id="06dd3-182">They are applied in order they are registered.</span></span>

<span data-ttu-id="06dd3-183">Cada chamada para `Configure<TOptions>` adiciona um `IConfigureOptions<TOptions>` serviço ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="06dd3-183">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="06dd3-184">No exemplo anterior, os valores de `Option1` e `Option2` estão especificados na *appSettings. JSON* –, mas o valor de `Option1` é substituído pelo delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="06dd3-184">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="06dd3-185">Quando mais de um serviço de configuração é habilitado, a última fonte de configuração especificado "wins" (define o valor de configuração).</span><span class="sxs-lookup"><span data-stu-id="06dd3-185">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="06dd3-186">No código anterior, o `HomeController.Index` método retornará `option1 = value1_from_action, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="06dd3-186">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="06dd3-187">Quando você associa opções de configuração, cada propriedade no seu tipo de opções está associada a uma chave de configuração do formulário `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="06dd3-187">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="06dd3-188">Por exemplo, o `MyOptions.Option1` propriedade está associada à chave `Option1`, que são lidos a partir de `option1` propriedade em *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="06dd3-188">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="06dd3-189">Um exemplo de subpropriedade é mostrado mais adiante neste artigo.</span><span class="sxs-lookup"><span data-stu-id="06dd3-189">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="06dd3-190">No código a seguir, um terceiro `IConfigureOptions<TOptions>` serviço é adicionado ao contêiner de serviço.</span><span class="sxs-lookup"><span data-stu-id="06dd3-190">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="06dd3-191">Ele é ligado `MySubOptions` a seção `subsection` do *appSettings. JSON* arquivo:</span><span class="sxs-lookup"><span data-stu-id="06dd3-191">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

<span data-ttu-id="06dd3-192">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-192">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]</span></span>

<span data-ttu-id="06dd3-193">Observação: Esse método de extensão exige o `Microsoft.Extensions.Options.ConfigurationExtensions` pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="06dd3-193">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="06dd3-194">Usando o seguinte *appSettings. JSON* arquivo:</span><span class="sxs-lookup"><span data-stu-id="06dd3-194">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="06dd3-195">[!code-json[Main](configuration/sample/src/UsingOptions/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-195">[!code-json[Main](configuration/sample/src/UsingOptions/appsettings.json)]</span></span>

<span data-ttu-id="06dd3-196">O `MySubOptions` classe:</span><span class="sxs-lookup"><span data-stu-id="06dd3-196">The `MySubOptions` class:</span></span>

<span data-ttu-id="06dd3-197">[!code-csharp[Main](configuration/sample/src/UsingOptions/Models/MySubOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-197">[!code-csharp[Main](configuration/sample/src/UsingOptions/Models/MySubOptions.cs)]</span></span>

<span data-ttu-id="06dd3-198">Com os seguintes `Controller`:</span><span class="sxs-lookup"><span data-stu-id="06dd3-198">With the following `Controller`:</span></span>

<span data-ttu-id="06dd3-199">[!code-csharp[Main](configuration/sample/src/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-199">[!code-csharp[Main](configuration/sample/src/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]</span></span>

<span data-ttu-id="06dd3-200">`subOption1 = subvalue1_from_json, subOption2 = 200`é retornado.</span><span class="sxs-lookup"><span data-stu-id="06dd3-200">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="06dd3-201">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="06dd3-201">IOptionsSnapshot</span></span>

<span data-ttu-id="06dd3-202">*Exige o ASP.NET Core 1.1 ou posterior.*</span><span class="sxs-lookup"><span data-stu-id="06dd3-202">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="06dd3-203">`IOptionsSnapshot`dá suporte a recarregar os dados de configuração quando o arquivo de configuração foi alterada.</span><span class="sxs-lookup"><span data-stu-id="06dd3-203">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="06dd3-204">Ele tem uma sobrecarga mínima.</span><span class="sxs-lookup"><span data-stu-id="06dd3-204">It has minimal overhead.</span></span> <span data-ttu-id="06dd3-205">Usando `IOptionsSnapshot` com `reloadOnChange: true`, as opções são vinculadas a `IConfiguration` e recarregados quando alterado.</span><span class="sxs-lookup"><span data-stu-id="06dd3-205">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="06dd3-206">O exemplo a seguir demonstra como um novo `IOptionsSnapshot` é criada após *config. JSON* alterações.</span><span class="sxs-lookup"><span data-stu-id="06dd3-206">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="06dd3-207">Solicitações para o servidor retornará o mesmo tempo quando *config. JSON* tem **não** alterado.</span><span class="sxs-lookup"><span data-stu-id="06dd3-207">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="06dd3-208">A primeira solicitação após *config. JSON* alterações mostrará uma nova hora.</span><span class="sxs-lookup"><span data-stu-id="06dd3-208">The first request after *config.json* changes will show a new time.</span></span>

<span data-ttu-id="06dd3-209">[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-209">[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]</span></span>

<span data-ttu-id="06dd3-210">A imagem a seguir mostra a saída do servidor:</span><span class="sxs-lookup"><span data-stu-id="06dd3-210">The following image shows the server output:</span></span>

![mostrando de imagem do navegador "última atualização: 22/11/2016 16:43:00"](configuration/_static/first.png)

<span data-ttu-id="06dd3-212">Atualizar o navegador não altera o valor de mensagem ou hora exibida (quando *config. JSON* não foi alterado).</span><span class="sxs-lookup"><span data-stu-id="06dd3-212">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="06dd3-213">Alterar e salvar o *config. JSON* e, em seguida, atualize o navegador:</span><span class="sxs-lookup"><span data-stu-id="06dd3-213">Change and save the  *config.json* and then refresh the browser:</span></span>

![mostrando de imagem do navegador "última atualização para e: 22/11/2016 4:53 PM"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="06dd3-215">Provedor na memória e a associação a uma classe POCO</span><span class="sxs-lookup"><span data-stu-id="06dd3-215">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="06dd3-216">O exemplo a seguir mostra como usar o provedor na memória e vincular a uma classe:</span><span class="sxs-lookup"><span data-stu-id="06dd3-216">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

<span data-ttu-id="06dd3-217">[!code-csharp[Main](configuration/sample/src/InMemory/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-217">[!code-csharp[Main](configuration/sample/src/InMemory/Program.cs)]</span></span>

<span data-ttu-id="06dd3-218">Os valores de configuração são retornados como cadeias de caracteres, mas a associação permite que a construção de objetos.</span><span class="sxs-lookup"><span data-stu-id="06dd3-218">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="06dd3-219">Associação permite que você recupere POCO objetos ou gráficos de objeto inteiro até mesmo.</span><span class="sxs-lookup"><span data-stu-id="06dd3-219">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="06dd3-220">O exemplo a seguir mostra como associar a `MyWindow` e usar o padrão de opções com um aplicativo MVC do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="06dd3-220">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

<span data-ttu-id="06dd3-221">[!code-csharp[Main](configuration/sample/src/WebConfigBind/MyWindow.cs)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-221">[!code-csharp[Main](configuration/sample/src/WebConfigBind/MyWindow.cs)]</span></span>

<span data-ttu-id="06dd3-222">[!code-json[Main](configuration/sample/src/WebConfigBind/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-222">[!code-json[Main](configuration/sample/src/WebConfigBind/appsettings.json)]</span></span>

<span data-ttu-id="06dd3-223">Associar a classe personalizada em `ConfigureServices` no `Startup` classe:</span><span class="sxs-lookup"><span data-stu-id="06dd3-223">Bind the custom class in `ConfigureServices` in the `Startup` class:</span></span>

<span data-ttu-id="06dd3-224">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Startup.cs?name=snippet1&highlight=3,4)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-224">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Startup.cs?name=snippet1&highlight=3,4)]</span></span>

<span data-ttu-id="06dd3-225">Exibir as configurações de `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="06dd3-225">Display the settings from the `HomeController`:</span></span>

<span data-ttu-id="06dd3-226">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Controllers/HomeController.cs)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-226">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Controllers/HomeController.cs)]</span></span>

### <a name="getvalue"></a><span data-ttu-id="06dd3-227">GetValue</span><span class="sxs-lookup"><span data-stu-id="06dd3-227">GetValue</span></span>

<span data-ttu-id="06dd3-228">O exemplo a seguir demonstra o [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) método de extensão:</span><span class="sxs-lookup"><span data-stu-id="06dd3-228">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

<span data-ttu-id="06dd3-229">[!code-csharp[Main](configuration/sample/src/InMemoryGetValue/Program.cs?highlight=27-29)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-229">[!code-csharp[Main](configuration/sample/src/InMemoryGetValue/Program.cs?highlight=27-29)]</span></span>

<span data-ttu-id="06dd3-230">O ConfigurationBinder `GetValue<T>` método permite que você especifique um valor padrão (80 no exemplo).</span><span class="sxs-lookup"><span data-stu-id="06dd3-230">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="06dd3-231">`GetValue<T>`para cenários simples e não vincular ao seções inteiras.</span><span class="sxs-lookup"><span data-stu-id="06dd3-231">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="06dd3-232">`GetValue<T>`Obtém os valores escalares de `GetSection(key).Value` convertido em um tipo específico.</span><span class="sxs-lookup"><span data-stu-id="06dd3-232">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="06dd3-233">Associando a um gráfico de objeto</span><span class="sxs-lookup"><span data-stu-id="06dd3-233">Binding to an object graph</span></span>

<span data-ttu-id="06dd3-234">Você pode vincular recursivamente para cada objeto em uma classe.</span><span class="sxs-lookup"><span data-stu-id="06dd3-234">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="06dd3-235">Considere o seguinte `AppOptions` classe:</span><span class="sxs-lookup"><span data-stu-id="06dd3-235">Consider the following `AppOptions` class:</span></span>

<span data-ttu-id="06dd3-236">[!code-csharp[Main](configuration/sample/src/ObjectGraph/AppOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-236">[!code-csharp[Main](configuration/sample/src/ObjectGraph/AppOptions.cs)]</span></span>

<span data-ttu-id="06dd3-237">O exemplo a seguir associa ao `AppOptions` classe:</span><span class="sxs-lookup"><span data-stu-id="06dd3-237">The following sample binds to the `AppOptions` class:</span></span>

<span data-ttu-id="06dd3-238">[!code-csharp[Main](configuration/sample/src/ObjectGraph/Program.cs?highlight=15-16)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-238">[!code-csharp[Main](configuration/sample/src/ObjectGraph/Program.cs?highlight=15-16)]</span></span>

<span data-ttu-id="06dd3-239">**ASP.NET Core 1.1** e superior podem usar `Get<T>`, que trabalha com seções inteiras.</span><span class="sxs-lookup"><span data-stu-id="06dd3-239">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="06dd3-240">`Get<T>`pode ser mais convienent que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="06dd3-240">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="06dd3-241">O código a seguir mostra como usar `Get<T>` com o exemplo acima:</span><span class="sxs-lookup"><span data-stu-id="06dd3-241">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="06dd3-242">Usando o seguinte *appSettings. JSON* arquivo:</span><span class="sxs-lookup"><span data-stu-id="06dd3-242">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="06dd3-243">[!code-json[Main](configuration/sample/src/ObjectGraph/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-243">[!code-json[Main](configuration/sample/src/ObjectGraph/appsettings.json)]</span></span>

<span data-ttu-id="06dd3-244">O programa exibe `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="06dd3-244">The program displays `Height 11`.</span></span>

<span data-ttu-id="06dd3-245">O código a seguir pode ser usado para a unidade a configuração de teste:</span><span class="sxs-lookup"><span data-stu-id="06dd3-245">The following code can be used to unit test the configuration:</span></span>

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

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="06dd3-246">Exemplo básico do provedor personalizado do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="06dd3-246">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="06dd3-247">Nesta seção, é criado um provedor de configuração básica que lê os pares nome-valor de um banco de dados usando EF.</span><span class="sxs-lookup"><span data-stu-id="06dd3-247">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="06dd3-248">Definir um `ConfigurationValue` entidade para armazenar valores de configuração no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="06dd3-248">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

<span data-ttu-id="06dd3-249">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/ConfigurationValue.cs)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-249">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/ConfigurationValue.cs)]</span></span>

<span data-ttu-id="06dd3-250">Adicionar um `ConfigurationContext` para armazenar e acessar os valores configurados:</span><span class="sxs-lookup"><span data-stu-id="06dd3-250">Add a `ConfigurationContext` to store and access the configured values:</span></span>

<span data-ttu-id="06dd3-251">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-251">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]</span></span>

<span data-ttu-id="06dd3-252">Criar uma classe que implementa [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="06dd3-252">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

<span data-ttu-id="06dd3-253">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-253">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]</span></span>

<span data-ttu-id="06dd3-254">Criar o provedor de configuração personalizado herdando de [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="06dd3-254">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="06dd3-255">O provedor de configuração inicializa o banco de dados quando ela estiver vazia:</span><span class="sxs-lookup"><span data-stu-id="06dd3-255">The configuration provider initializes the database when it's empty:</span></span>

<span data-ttu-id="06dd3-256">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-256">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]</span></span>

<span data-ttu-id="06dd3-257">Os valores realçados do banco de dados ("value_from_ef_1" e "value_from_ef_2") são exibidos quando o exemplo for executado.</span><span class="sxs-lookup"><span data-stu-id="06dd3-257">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="06dd3-258">Você pode adicionar um `EFConfigSource` método de extensão para adicionar a fonte de configuração:</span><span class="sxs-lookup"><span data-stu-id="06dd3-258">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

<span data-ttu-id="06dd3-259">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-259">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]</span></span>

<span data-ttu-id="06dd3-260">O código a seguir mostra como usar personalizado `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="06dd3-260">The following code shows how to use the custom `EFConfigProvider`:</span></span>

<span data-ttu-id="06dd3-261">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/Program.cs?highlight=20-25)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-261">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/Program.cs?highlight=20-25)]</span></span>

<span data-ttu-id="06dd3-262">Observe que o exemplo adiciona personalizado `EFConfigProvider` depois que o provedor JSON, então todas as configurações do banco de dados irá substituir as configurações do *appSettings. JSON* arquivo.</span><span class="sxs-lookup"><span data-stu-id="06dd3-262">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="06dd3-263">Usando o seguinte *appSettings. JSON* arquivo:</span><span class="sxs-lookup"><span data-stu-id="06dd3-263">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="06dd3-264">[!code-json[Main](configuration/sample/src/CustomConfigurationProvider/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-264">[!code-json[Main](configuration/sample/src/CustomConfigurationProvider/appsettings.json)]</span></span>

<span data-ttu-id="06dd3-265">O seguinte é exibido:</span><span class="sxs-lookup"><span data-stu-id="06dd3-265">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="06dd3-266">Provedor de configuração de linha de comando</span><span class="sxs-lookup"><span data-stu-id="06dd3-266">CommandLine configuration provider</span></span>

<span data-ttu-id="06dd3-267">O exemplo a seguir habilita o provedor de configuração de linha de comando último:</span><span class="sxs-lookup"><span data-stu-id="06dd3-267">The following sample enables the CommandLine configuration provider last:</span></span>

<span data-ttu-id="06dd3-268">[!code-csharp[Main](configuration/sample/src/CommandLine/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="06dd3-268">[!code-csharp[Main](configuration/sample/src/CommandLine/Program.cs)]</span></span>

<span data-ttu-id="06dd3-269">Use o seguinte para passar parâmetros de configuração:</span><span class="sxs-lookup"><span data-stu-id="06dd3-269">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=Bob /App:MainWindow:Left=1234
```

<span data-ttu-id="06dd3-270">Que exibe:</span><span class="sxs-lookup"><span data-stu-id="06dd3-270">Which displays:</span></span>

```console
Hello Bob
Left 1234
```

<span data-ttu-id="06dd3-271">O `GetSwitchMappings` método permite que você use `-` em vez de `/` e ele extrai os prefixos de subchave à esquerda.</span><span class="sxs-lookup"><span data-stu-id="06dd3-271">The `GetSwitchMappings` method allows you to use `-` rather than `/` and it strips the leading subkey prefixes.</span></span> <span data-ttu-id="06dd3-272">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="06dd3-272">For example:</span></span>

```console
dotnet run -MachineName=Bob -Left=7734
```

<span data-ttu-id="06dd3-273">Exibe:</span><span class="sxs-lookup"><span data-stu-id="06dd3-273">Displays:</span></span>

```console
Hello Bob
Left 7734
```

<span data-ttu-id="06dd3-274">Argumentos de linha de comando devem incluir um valor (pode ser nulo).</span><span class="sxs-lookup"><span data-stu-id="06dd3-274">Command-line arguments must include a value (it can be null).</span></span> <span data-ttu-id="06dd3-275">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="06dd3-275">For example:</span></span>

```console
dotnet run /Profile:MachineName=
```

<span data-ttu-id="06dd3-276">É Okey, mas</span><span class="sxs-lookup"><span data-stu-id="06dd3-276">Is OK, but</span></span>

```console
dotnet run /Profile:MachineName
```

<span data-ttu-id="06dd3-277">resulta em uma exceção.</span><span class="sxs-lookup"><span data-stu-id="06dd3-277">results in an exception.</span></span> <span data-ttu-id="06dd3-278">Uma exceção será lançada se você especificar um prefixo de linha de comando de - ou -- para o qual não há nenhum mapeamento correspondente do comutador.</span><span class="sxs-lookup"><span data-stu-id="06dd3-278">An exception will be thrown if you specify a command-line switch prefix of - or -- for which there's no corresponding switch mapping.</span></span>

## <a name="the-webconfig-file"></a><span data-ttu-id="06dd3-279">O arquivo Web. config</span><span class="sxs-lookup"><span data-stu-id="06dd3-279">The web.config file</span></span>

<span data-ttu-id="06dd3-280">Um *Web. config* arquivo é necessário quando você hospeda o aplicativo no IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="06dd3-280">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="06dd3-281">*Web. config* ativa AspNetCoreModule no IIS para iniciar seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="06dd3-281">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="06dd3-282">As configurações no *Web. config* habilitar AspNetCoreModule no IIS para iniciar seu aplicativo e definir outras configurações do IIS e módulos.</span><span class="sxs-lookup"><span data-stu-id="06dd3-282">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="06dd3-283">Se você estiver usando o Visual Studio e excluir *Web. config*, Visual Studio irá criar um novo.</span><span class="sxs-lookup"><span data-stu-id="06dd3-283">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="06dd3-284">Observações adicionais</span><span class="sxs-lookup"><span data-stu-id="06dd3-284">Additional notes</span></span>

* <span data-ttu-id="06dd3-285">Injeção de dependência (DI) não está definido até depois `ConfigureServices` é invocado.</span><span class="sxs-lookup"><span data-stu-id="06dd3-285">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="06dd3-286">O sistema de configuração não está ciente de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="06dd3-286">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="06dd3-287">`IConfiguration`tem dois especializações:</span><span class="sxs-lookup"><span data-stu-id="06dd3-287">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="06dd3-288">`IConfigurationRoot`Usado para o nó raiz.</span><span class="sxs-lookup"><span data-stu-id="06dd3-288">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="06dd3-289">Pode disparar um recarregamento.</span><span class="sxs-lookup"><span data-stu-id="06dd3-289">Can trigger a reload.</span></span>
  * <span data-ttu-id="06dd3-290">`IConfigurationSection`Representa uma seção de valores de configuração.</span><span class="sxs-lookup"><span data-stu-id="06dd3-290">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="06dd3-291">O `GetSection` e `GetChildren` métodos retornam um `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="06dd3-291">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="06dd3-292">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="06dd3-292">Additional resources</span></span>

* [<span data-ttu-id="06dd3-293">Trabalhando com vários ambientes</span><span class="sxs-lookup"><span data-stu-id="06dd3-293">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="06dd3-294">Armazenamento seguro de segredos do aplicativo durante o desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="06dd3-294">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="06dd3-295">Injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="06dd3-295">Dependency Injection</span></span>](dependency-injection.md)
* [<span data-ttu-id="06dd3-296">Provedor de configuração do Cofre de chaves do Azure</span><span class="sxs-lookup"><span data-stu-id="06dd3-296">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)

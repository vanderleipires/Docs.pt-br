---
title: "Configuração no ASP.NET Core"
author: rick-anderson
description: "Use a API de configuração para configurar um aplicativo do ASP.NET Core por vários métodos."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: 6281d6ba254670b111964715410fc0694ae4d149
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2017
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="939a6-103">Configurar um aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="939a6-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="939a6-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="939a6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="939a6-105">A API de configuração fornece uma maneira de configurar um aplicativo Web do ASP.NET Core com base em uma lista de pares nome-valor.</span><span class="sxs-lookup"><span data-stu-id="939a6-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="939a6-106">A configuração é lida em tempo de execução por meio de várias fontes.</span><span class="sxs-lookup"><span data-stu-id="939a6-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="939a6-107">Você pode agrupar esses pares de nome-valor em uma hierarquia de vários níveis.</span><span class="sxs-lookup"><span data-stu-id="939a6-107">You can group these name-value pairs into a multi-level hierarchy.</span></span> 

<span data-ttu-id="939a6-108">Há provedores de configuração para:</span><span class="sxs-lookup"><span data-stu-id="939a6-108">There are configuration providers for:</span></span>

* <span data-ttu-id="939a6-109">Formatos de arquivo (INI, JSON e XML)</span><span class="sxs-lookup"><span data-stu-id="939a6-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="939a6-110">Argumentos de linha de comando</span><span class="sxs-lookup"><span data-stu-id="939a6-110">Command-line arguments</span></span>
* <span data-ttu-id="939a6-111">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="939a6-111">Environment variables</span></span>
* <span data-ttu-id="939a6-112">Objetos do .NET na memória</span><span class="sxs-lookup"><span data-stu-id="939a6-112">In-memory .NET objects</span></span>
* <span data-ttu-id="939a6-113">Um repositório de usuário criptografado</span><span class="sxs-lookup"><span data-stu-id="939a6-113">An encrypted user store</span></span>
* [<span data-ttu-id="939a6-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="939a6-114">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="939a6-115">Provedores personalizados (instalados ou criados)</span><span class="sxs-lookup"><span data-stu-id="939a6-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="939a6-116">Cada valor de configuração é mapeado para uma chave de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="939a6-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="939a6-117">Há suporte interno de associação para desserializar configurações em um objeto [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) personalizado (uma classe simples de .NET com propriedades).</span><span class="sxs-lookup"><span data-stu-id="939a6-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="939a6-118">O padrão de opções usa classes de opções para representar grupos de configurações relacionadas.</span><span class="sxs-lookup"><span data-stu-id="939a6-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="939a6-119">Para obter mais informações sobre como usar o padrão de opções, consulte o tópico [Opções](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="939a6-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="939a6-120">[Exibir ou baixar código de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="939a6-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="939a6-121">Configuração de JSON</span><span class="sxs-lookup"><span data-stu-id="939a6-121">JSON configuration</span></span>

<span data-ttu-id="939a6-122">O aplicativo de console a seguir usa o provedor de configuração JSON:</span><span class="sxs-lookup"><span data-stu-id="939a6-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="939a6-123">O aplicativo lê e exibe as seguintes definições de configuração:</span><span class="sxs-lookup"><span data-stu-id="939a6-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="939a6-124">A configuração consiste em uma lista hierárquica de pares nome-valor na qual os nós são separados por dois pontos.</span><span class="sxs-lookup"><span data-stu-id="939a6-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="939a6-125">Para recuperar um valor, acesse o indexador `Configuration` com a chave do item correspondente:</span><span class="sxs-lookup"><span data-stu-id="939a6-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="939a6-126">Para trabalhar com matrizes em fontes de configuração formatadas em JSON, use um índice de matriz como parte da cadeia de caracteres separada por dois pontos.</span><span class="sxs-lookup"><span data-stu-id="939a6-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="939a6-127">O exemplo a seguir obtém o nome do primeiro item na matriz `wizards` anterior:</span><span class="sxs-lookup"><span data-stu-id="939a6-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="939a6-128">Os pares nome-valor gravados nos provedores `Configuration` internos **não** são mantidos.</span><span class="sxs-lookup"><span data-stu-id="939a6-128">Name-value pairs written to the built-in `Configuration` providers are **not** persisted.</span></span> <span data-ttu-id="939a6-129">No entanto, você pode criar um provedor personalizado que salva valores.</span><span class="sxs-lookup"><span data-stu-id="939a6-129">However, you can create a custom provider that saves values.</span></span> <span data-ttu-id="939a6-130">Consulte [provedor de configuração personalizado](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="939a6-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="939a6-131">O exemplo anterior usa o indexador de configuração para ler valores.</span><span class="sxs-lookup"><span data-stu-id="939a6-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="939a6-132">Para acessar a configuração fora de `Startup`, use o *padrão de opções*.</span><span class="sxs-lookup"><span data-stu-id="939a6-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="939a6-133">Para obter mais informações, consulte o tópico [Opções](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="939a6-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="939a6-134">É comum ter configurações diferentes para ambientes diferentes, por exemplo, desenvolvimento, teste e produção.</span><span class="sxs-lookup"><span data-stu-id="939a6-134">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="939a6-135">O método de extensão `CreateDefaultBuilder` em um aplicativo do ASP.NET Core 2.x (ou usando `AddJsonFile` e `AddEnvironmentVariables` diretamente em um aplicativo do ASP.NET Core 1.x) adiciona provedores de configuração para ler arquivos JSON e fontes de configuração do sistema:</span><span class="sxs-lookup"><span data-stu-id="939a6-135">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="939a6-136">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="939a6-136">*appsettings.json*</span></span>
* <span data-ttu-id="939a6-137">*appsettings.\<EnvironmentName>.json*</span><span class="sxs-lookup"><span data-stu-id="939a6-137">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="939a6-138">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="939a6-138">Environment variables</span></span>

<span data-ttu-id="939a6-139">Consulte [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) para uma explicação sobre os parâmetros.</span><span class="sxs-lookup"><span data-stu-id="939a6-139">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="939a6-140">`reloadOnChange` só é compatível no ASP.NET Core 1.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="939a6-140">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span> 

<span data-ttu-id="939a6-141">As fontes de configuração são lidas na ordem em que são especificadas.</span><span class="sxs-lookup"><span data-stu-id="939a6-141">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="939a6-142">No código acima, as variáveis de ambiente são lidas por último.</span><span class="sxs-lookup"><span data-stu-id="939a6-142">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="939a6-143">Os valores de configuração definidos por meio do ambiente substituem aqueles definidos nos dois provedores anteriores.</span><span class="sxs-lookup"><span data-stu-id="939a6-143">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="939a6-144">O ambiente normalmente é definido como `Development`, `Staging` ou `Production`.</span><span class="sxs-lookup"><span data-stu-id="939a6-144">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="939a6-145">Consulte [Trabalhando com vários ambientes](xref:fundamentals/environments) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="939a6-145">See [Working with multiple environments](xref:fundamentals/environments) for more information.</span></span>

<span data-ttu-id="939a6-146">Considerações de configuração:</span><span class="sxs-lookup"><span data-stu-id="939a6-146">Configuration considerations:</span></span>

* <span data-ttu-id="939a6-147">`IOptionsSnapshot` pode recarregar dados de configuração quando é alterado.</span><span class="sxs-lookup"><span data-stu-id="939a6-147">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="939a6-148">Consulte [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="939a6-148">See [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="939a6-149">As chaves de configuração não diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="939a6-149">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="939a6-150">Especifique as variáveis de ambiente por último para que o ambiente local possa substituir as configurações nos arquivos de configuração implantados.</span><span class="sxs-lookup"><span data-stu-id="939a6-150">Specify environment variables last so that the local environment can override settings in deployed configuration files.</span></span>
* <span data-ttu-id="939a6-151">**Nunca** armazene senhas ou outros dados confidenciais no código do provedor de configuração ou nos arquivos de configuração de texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="939a6-151">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="939a6-152">Não use segredos de produção em seus ambientes de teste ou de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="939a6-152">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="939a6-153">Em vez disso, especifique segredos fora do projeto para que eles não sejam acidentalmente comprometidos com seu repositório.</span><span class="sxs-lookup"><span data-stu-id="939a6-153">Instead, specify secrets outside of the project so that they can't be accidentally committed to your repository.</span></span> <span data-ttu-id="939a6-154">Saiba mais sobre [trabalhando com vários ambientes](xref:fundamentals/environments) e gerenciamento de [armazenamento seguro de segredos de aplicativo durante o desenvolvimento](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="939a6-154">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="939a6-155">Se dois pontos (`:`) não puder ser usado em variáveis de ambiente em seu sistema, substitua os dois pontos (`:`) por um sublinhado duplo (`__`).</span><span class="sxs-lookup"><span data-stu-id="939a6-155">If a colon (`:`) can't be used in environment variables on your system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="939a6-156">Provedor na memória e associação a uma classe POCO</span><span class="sxs-lookup"><span data-stu-id="939a6-156">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="939a6-157">O exemplo a seguir mostra como usar o provedor na memória e associar a uma classe:</span><span class="sxs-lookup"><span data-stu-id="939a6-157">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="939a6-158">Os valores de configuração são retornados como cadeias de caracteres, mas a associação permite a construção de objetos.</span><span class="sxs-lookup"><span data-stu-id="939a6-158">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="939a6-159">A associação permite que você recupere objetos POCO ou até mesmo gráficos de objetos inteiros.</span><span class="sxs-lookup"><span data-stu-id="939a6-159">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="939a6-160">GetValue</span><span class="sxs-lookup"><span data-stu-id="939a6-160">GetValue</span></span>

<span data-ttu-id="939a6-161">O exemplo a seguir demonstra o método de extensão [GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_):</span><span class="sxs-lookup"><span data-stu-id="939a6-161">The following sample demonstrates the [GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

<span data-ttu-id="939a6-162">O método `GetValue<T>` do ConfigurationBinder permite que você especifique um valor padrão (80 no exemplo).</span><span class="sxs-lookup"><span data-stu-id="939a6-162">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="939a6-163">`GetValue<T>` é para cenários simples e não se associa a seções inteiras.</span><span class="sxs-lookup"><span data-stu-id="939a6-163">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="939a6-164">`GetValue<T>` obtém valores escalares de `GetSection(key).Value` convertidos em um tipo específico.</span><span class="sxs-lookup"><span data-stu-id="939a6-164">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="939a6-165">Associar a um gráfico de objeto</span><span class="sxs-lookup"><span data-stu-id="939a6-165">Bind to an object graph</span></span>

<span data-ttu-id="939a6-166">Você pode associar recursivamente a cada objeto em uma classe.</span><span class="sxs-lookup"><span data-stu-id="939a6-166">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="939a6-167">Considere a classe `AppSettings` a seguir:</span><span class="sxs-lookup"><span data-stu-id="939a6-167">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="939a6-168">O exemplo a seguir associa à classe `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="939a6-168">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="939a6-169">O **ASP.NET Core 1.1** e superior pode usar `Get<T>`, que trabalha com seções inteiras.</span><span class="sxs-lookup"><span data-stu-id="939a6-169">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="939a6-170">O `Get<T>` pode ser mais conveniente do que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="939a6-170">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="939a6-171">O código a seguir mostra como usar `Get<T>` com o exemplo acima:</span><span class="sxs-lookup"><span data-stu-id="939a6-171">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="939a6-172">Usando o seguinte arquivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="939a6-172">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="939a6-173">O programa exibe `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="939a6-173">The program displays `Height 11`.</span></span>

<span data-ttu-id="939a6-174">O código a seguir pode ser usado para o teste de unidade da configuração:</span><span class="sxs-lookup"><span data-stu-id="939a6-174">The following code can be used to unit test the configuration:</span></span>

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

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="939a6-175">Criar um provedor personalizado do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="939a6-175">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="939a6-176">Nesta seção, é criado um provedor de configuração básico, que lê os pares nome-valor de um banco de dados, usando o EF.</span><span class="sxs-lookup"><span data-stu-id="939a6-176">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="939a6-177">Defina uma entidade `ConfigurationValue` para armazenar valores de configuração no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="939a6-177">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="939a6-178">Adicione um `ConfigurationContext` para armazenar e acessar os valores configurados:</span><span class="sxs-lookup"><span data-stu-id="939a6-178">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="939a6-179">Crie uma classe que implementa [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="939a6-179">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="939a6-180">Crie o provedor de configuração personalizado através da herança do [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="939a6-180">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="939a6-181">O provedor de configuração inicializa o banco de dados quando ele está vazio:</span><span class="sxs-lookup"><span data-stu-id="939a6-181">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="939a6-182">Os valores realçados do banco de dados ("value_from_ef_1" e "value_from_ef_2") são exibidos quando o exemplo é executado.</span><span class="sxs-lookup"><span data-stu-id="939a6-182">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="939a6-183">Você pode adicionar um método de extensão `EFConfigSource` para adicionar a fonte de configuração:</span><span class="sxs-lookup"><span data-stu-id="939a6-183">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="939a6-184">O código a seguir mostra como usar o `EFConfigProvider` personalizado:</span><span class="sxs-lookup"><span data-stu-id="939a6-184">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="939a6-185">Observe que o exemplo adiciona o `EFConfigProvider` personalizado depois do provedor JSON, de maneira que todas as configurações do banco de dados substituam as configurações do arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="939a6-185">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="939a6-186">Usando o seguinte arquivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="939a6-186">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="939a6-187">O seguinte é exibido:</span><span class="sxs-lookup"><span data-stu-id="939a6-187">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="939a6-188">Provedor de configuração CommandLine</span><span class="sxs-lookup"><span data-stu-id="939a6-188">CommandLine configuration provider</span></span>

<span data-ttu-id="939a6-189">O [Provedor de configuração CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) recebe pares chave-valor de argumento de linha de comando para a configuração em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="939a6-189">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="939a6-190">Exibir ou baixar o exemplo de configuração do CommandLine</span><span class="sxs-lookup"><span data-stu-id="939a6-190">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="939a6-191">Configurar e usar o provedor de configuração CommandLine</span><span class="sxs-lookup"><span data-stu-id="939a6-191">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="939a6-192">Configuração básica</span><span class="sxs-lookup"><span data-stu-id="939a6-192">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="939a6-193">Para ativar a configuração de linha de comando, chame o método de extensão `AddCommandLine` em uma instância do [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="939a6-193">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="939a6-194">Ao executar o código, a seguinte saída será exibida:</span><span class="sxs-lookup"><span data-stu-id="939a6-194">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="939a6-195">Passar pares chave-valor de argumento na linha de comando altera os valores de `Profile:MachineName` e `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="939a6-195">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="939a6-196">A janela do console exibe:</span><span class="sxs-lookup"><span data-stu-id="939a6-196">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="939a6-197">Para substituir a configuração fornecida por outros provedores de configuração pela configuração de linha de comando, chame `AddCommandLine` por último em `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="939a6-197">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="939a6-198">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="939a6-198">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="939a6-199">Os aplicativos típicos de ASP.NET Core 2.x usam o método de conveniência estático `CreateDefaultBuilder` para criar o host:</span><span class="sxs-lookup"><span data-stu-id="939a6-199">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="939a6-200">O `CreateDefaultBuilder` carrega a configuração opcional de *appsettings.json*, de *appsettings.{Environment}.json*, dos [segredos do usuário](xref:security/app-secrets) (no ambiente `Development`), das variáveis de ambiente e dos argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="939a6-200">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="939a6-201">O provedor CommandLine é chamado por último.</span><span class="sxs-lookup"><span data-stu-id="939a6-201">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="939a6-202">Chamar o provedor por último permite que os argumentos de linha de comando passados em tempo de execução substituam a configuração definida por outros provedores de configuração chamados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="939a6-202">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="939a6-203">Observe que para arquivos *appsettings* esse `reloadOnChange` está habilitado.</span><span class="sxs-lookup"><span data-stu-id="939a6-203">Note that for *appsettings* files that `reloadOnChange` is enabled.</span></span> <span data-ttu-id="939a6-204">Os argumentos de linha de comando são substituídos se um valor de configuração correspondente em um arquivo *appsettings* é alterado depois que o aplicativo é iniciado.</span><span class="sxs-lookup"><span data-stu-id="939a6-204">Command-line arguments are overridden if a matching configuration value in an *appsettings* file is changed after the app starts.</span></span>

> [!NOTE]
> <span data-ttu-id="939a6-205">Como uma alternativa ao uso do método `CreateDefaultBuilder`, a criação de um host usando [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) e a criação manual da configuração com [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) são compatíveis no ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="939a6-205">As an alternative to using the `CreateDefaultBuilder` method, creating a host using [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) and manually building configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) is supported in ASP.NET Core 2.x.</span></span> <span data-ttu-id="939a6-206">Consulte a guia do ASP.NET Core 1.x para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="939a6-206">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="939a6-207">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="939a6-207">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="939a6-208">Crie um [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) e chame o método `AddCommandLine` para usar o provedor de configuração CommandLine.</span><span class="sxs-lookup"><span data-stu-id="939a6-208">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="939a6-209">Chamar o provedor por último permite que os argumentos de linha de comando passados em tempo de execução substituam a configuração definida por outros provedores de configuração chamados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="939a6-209">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="939a6-210">Aplique a configuração ao [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) com o método `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="939a6-210">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="939a6-211">Arguments</span><span class="sxs-lookup"><span data-stu-id="939a6-211">Arguments</span></span>

<span data-ttu-id="939a6-212">Os argumentos passados na linha de comando devem estar em conformidade com um dos dois formatos mostrados na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="939a6-212">Arguments passed on the command line must conform to one of two formats shown in the following table.</span></span>

| <span data-ttu-id="939a6-213">Formato de argumento</span><span class="sxs-lookup"><span data-stu-id="939a6-213">Argument format</span></span>                                                     | <span data-ttu-id="939a6-214">Exemplo</span><span class="sxs-lookup"><span data-stu-id="939a6-214">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="939a6-215">Argumento único: um par chave-valor separado por um sinal de igual (`=`)</span><span class="sxs-lookup"><span data-stu-id="939a6-215">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="939a6-216">Sequência de dois argumentos: um par chave-valor separado por um espaço</span><span class="sxs-lookup"><span data-stu-id="939a6-216">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="939a6-217">**Argumento único**</span><span class="sxs-lookup"><span data-stu-id="939a6-217">**Single argument**</span></span>

<span data-ttu-id="939a6-218">O valor deve seguir um sinal de igual (`=`).</span><span class="sxs-lookup"><span data-stu-id="939a6-218">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="939a6-219">O valor pode ser nulo (por exemplo, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="939a6-219">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="939a6-220">A chave pode ter um prefixo.</span><span class="sxs-lookup"><span data-stu-id="939a6-220">The key may have a prefix.</span></span>

| <span data-ttu-id="939a6-221">Prefixo da chave</span><span class="sxs-lookup"><span data-stu-id="939a6-221">Key prefix</span></span>               | <span data-ttu-id="939a6-222">Exemplo</span><span class="sxs-lookup"><span data-stu-id="939a6-222">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="939a6-223">Nenhum prefixo</span><span class="sxs-lookup"><span data-stu-id="939a6-223">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="939a6-224">Traço único (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="939a6-224">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="939a6-225">Dois traços (`--`)</span><span class="sxs-lookup"><span data-stu-id="939a6-225">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="939a6-226">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="939a6-226">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="939a6-227">&#8224;Uma chave com um prefixo de traço único (`-`) deve ser fornecida em [mapeamentos de comutador](#switch-mappings), como descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="939a6-227">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="939a6-228">Comando de exemplo:</span><span class="sxs-lookup"><span data-stu-id="939a6-228">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="939a6-229">Observação: se `-key1` não estiver presente nos [mapeamentos de comutador](#switch-mappings) fornecidos para o provedor de configuração, uma `FormatException` será gerada.</span><span class="sxs-lookup"><span data-stu-id="939a6-229">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="939a6-230">**Sequência de dois argumentos**</span><span class="sxs-lookup"><span data-stu-id="939a6-230">**Sequence of two arguments**</span></span>

<span data-ttu-id="939a6-231">O valor não pode ser nulo e deve seguir a chave separada por um espaço.</span><span class="sxs-lookup"><span data-stu-id="939a6-231">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="939a6-232">A chave deve ter um prefixo.</span><span class="sxs-lookup"><span data-stu-id="939a6-232">The key must have a prefix.</span></span>

| <span data-ttu-id="939a6-233">Prefixo da chave</span><span class="sxs-lookup"><span data-stu-id="939a6-233">Key prefix</span></span>               | <span data-ttu-id="939a6-234">Exemplo</span><span class="sxs-lookup"><span data-stu-id="939a6-234">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="939a6-235">Traço único (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="939a6-235">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="939a6-236">Dois traços (`--`)</span><span class="sxs-lookup"><span data-stu-id="939a6-236">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="939a6-237">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="939a6-237">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="939a6-238">&#8224;Uma chave com um prefixo de traço único (`-`) deve ser fornecida em [mapeamentos de comutador](#switch-mappings), como descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="939a6-238">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="939a6-239">Comando de exemplo:</span><span class="sxs-lookup"><span data-stu-id="939a6-239">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="939a6-240">Observação: se `-key1` não estiver presente nos [mapeamentos de comutador](#switch-mappings) fornecidos para o provedor de configuração, uma `FormatException` será gerada.</span><span class="sxs-lookup"><span data-stu-id="939a6-240">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="939a6-241">Chaves duplicadas</span><span class="sxs-lookup"><span data-stu-id="939a6-241">Duplicate keys</span></span>

<span data-ttu-id="939a6-242">Se chaves duplicadas forem fornecidas, o último par chave-valor será usado.</span><span class="sxs-lookup"><span data-stu-id="939a6-242">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="939a6-243">Mapeamentos de comutador</span><span class="sxs-lookup"><span data-stu-id="939a6-243">Switch mappings</span></span>

<span data-ttu-id="939a6-244">Ao criar manualmente a configuração com `ConfigurationBuilder`, você pode fornecer opcionalmente um dicionário de mapeamentos de comutador para o método `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="939a6-244">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="939a6-245">Os mapeamentos de comutador permitem que você forneça a lógica de substituição do nome da chave.</span><span class="sxs-lookup"><span data-stu-id="939a6-245">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="939a6-246">Ao ser usado, o dicionário de mapeamentos de comutador é verificado para oferecer uma chave que corresponda à chave fornecida por um argumento de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="939a6-246">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="939a6-247">Se a chave de linha de comando for encontrada no dicionário, o valor do dicionário (a substituição da chave) será passado de volta para definir a configuração.</span><span class="sxs-lookup"><span data-stu-id="939a6-247">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="939a6-248">Um mapeamento de comutador é necessário para qualquer chave de linha de comando prefixada com um traço único (`-`).</span><span class="sxs-lookup"><span data-stu-id="939a6-248">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="939a6-249">Regras de chave do dicionário de mapeamentos de comutador:</span><span class="sxs-lookup"><span data-stu-id="939a6-249">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="939a6-250">Os comutadores devem começar com um traço (`-`) ou traço duplo (`--`).</span><span class="sxs-lookup"><span data-stu-id="939a6-250">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="939a6-251">O dicionário de mapeamentos de comutador chave não deve conter chaves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="939a6-251">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="939a6-252">No exemplo a seguir, o método `GetSwitchMappings` permite que seus argumentos de linha de comando usem um prefixo de chave de traço único (`-`) e evitem prefixos de subchaves à esquerda.</span><span class="sxs-lookup"><span data-stu-id="939a6-252">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="939a6-253">Sem fornecer argumentos de linha de comando, o dicionário fornecido para `AddInMemoryCollection` definirá os valores de configuração.</span><span class="sxs-lookup"><span data-stu-id="939a6-253">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="939a6-254">Execute o aplicativo com o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="939a6-254">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="939a6-255">A janela do console exibe:</span><span class="sxs-lookup"><span data-stu-id="939a6-255">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="939a6-256">Use o seguinte para passar parâmetros de configuração:</span><span class="sxs-lookup"><span data-stu-id="939a6-256">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="939a6-257">A janela do console exibe:</span><span class="sxs-lookup"><span data-stu-id="939a6-257">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="939a6-258">Depois que o dicionário de mapeamentos de comutador for criado, ele conterá os dados mostrados na tabela a seguir.</span><span class="sxs-lookup"><span data-stu-id="939a6-258">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="939a6-259">Chave</span><span class="sxs-lookup"><span data-stu-id="939a6-259">Key</span></span>            | <span data-ttu-id="939a6-260">Valor</span><span class="sxs-lookup"><span data-stu-id="939a6-260">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="939a6-261">Para demonstrar a troca de chaves usando o dicionário, execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="939a6-261">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="939a6-262">As chaves da linha de comando são trocadas.</span><span class="sxs-lookup"><span data-stu-id="939a6-262">The command-line keys are swapped.</span></span> <span data-ttu-id="939a6-263">A janela de console exibe os valores de configuração de `Profile:MachineName` e `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="939a6-263">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="939a6-264">O arquivo Web.config</span><span class="sxs-lookup"><span data-stu-id="939a6-264">The web.config file</span></span>

<span data-ttu-id="939a6-265">Um arquivo *Web.config* é necessário quando você hospeda o aplicativo no IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="939a6-265">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="939a6-266">O *Web.config* ativa o AspNetCoreModule no IIS para iniciar seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="939a6-266">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="939a6-267">As configurações no *Web.config* habilitam o AspNetCoreModule no IIS para iniciar seu aplicativo e definir outras configurações e módulos do IIS.</span><span class="sxs-lookup"><span data-stu-id="939a6-267">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="939a6-268">Se você estiver usando o Visual Studio e excluir o *Web.config*, o Visual Studio criará um novo.</span><span class="sxs-lookup"><span data-stu-id="939a6-268">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="939a6-269">Observações adicionais</span><span class="sxs-lookup"><span data-stu-id="939a6-269">Additional notes</span></span>

* <span data-ttu-id="939a6-270">A DI (injeção de dependência) não é configurada até que `ConfigureServices` seja invocado.</span><span class="sxs-lookup"><span data-stu-id="939a6-270">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="939a6-271">O sistema de configuração não tem reconhecimento de DI.</span><span class="sxs-lookup"><span data-stu-id="939a6-271">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="939a6-272">O `IConfiguration` tem duas especializações:</span><span class="sxs-lookup"><span data-stu-id="939a6-272">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="939a6-273">`IConfigurationRoot` Usada para o nó raiz.</span><span class="sxs-lookup"><span data-stu-id="939a6-273">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="939a6-274">Pode disparar um recarregamento.</span><span class="sxs-lookup"><span data-stu-id="939a6-274">Can trigger a reload.</span></span>
  * <span data-ttu-id="939a6-275">`IConfigurationSection` Representa uma seção de valores de configuração.</span><span class="sxs-lookup"><span data-stu-id="939a6-275">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="939a6-276">O métodos `GetSection` e `GetChildren` retornam um `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="939a6-276">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="939a6-277">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="939a6-277">Additional resources</span></span>

* [<span data-ttu-id="939a6-278">Opções</span><span class="sxs-lookup"><span data-stu-id="939a6-278">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="939a6-279">Trabalhando com vários ambientes</span><span class="sxs-lookup"><span data-stu-id="939a6-279">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="939a6-280">Armazenamento seguro dos segredos do aplicativo durante o desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="939a6-280">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="939a6-281">Hospedagem no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="939a6-281">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="939a6-282">Injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="939a6-282">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="939a6-283">Provedor de configuração do Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="939a6-283">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)

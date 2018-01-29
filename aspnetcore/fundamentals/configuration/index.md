---
title: "Configuração no ASP.NET Core"
author: rick-anderson
description: "Use a API de configuração para configurar um aplicativo do ASP.NET Core por vários métodos."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: ee9bdc66d0bfa6433736fbc55126bdd37ba9d080
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="fbe94-103">Configurar um aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fbe94-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="fbe94-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fbe94-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fbe94-105">A API de configuração fornece uma maneira de configurar um aplicativo Web do ASP.NET Core com base em uma lista de pares nome-valor.</span><span class="sxs-lookup"><span data-stu-id="fbe94-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="fbe94-106">A configuração é lida em tempo de execução por meio de várias fontes.</span><span class="sxs-lookup"><span data-stu-id="fbe94-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="fbe94-107">Você pode agrupar esses pares de nome-valor em uma hierarquia de vários níveis.</span><span class="sxs-lookup"><span data-stu-id="fbe94-107">You can group these name-value pairs into a multi-level hierarchy.</span></span>

<span data-ttu-id="fbe94-108">Há provedores de configuração para:</span><span class="sxs-lookup"><span data-stu-id="fbe94-108">There are configuration providers for:</span></span>

* <span data-ttu-id="fbe94-109">Formatos de arquivo (INI, JSON e XML)</span><span class="sxs-lookup"><span data-stu-id="fbe94-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="fbe94-110">Argumentos de linha de comando</span><span class="sxs-lookup"><span data-stu-id="fbe94-110">Command-line arguments</span></span>
* <span data-ttu-id="fbe94-111">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="fbe94-111">Environment variables</span></span>
* <span data-ttu-id="fbe94-112">Objetos do .NET na memória</span><span class="sxs-lookup"><span data-stu-id="fbe94-112">In-memory .NET objects</span></span>
* <span data-ttu-id="fbe94-113">Um repositório de usuário criptografado</span><span class="sxs-lookup"><span data-stu-id="fbe94-113">An encrypted user store</span></span>
* [<span data-ttu-id="fbe94-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="fbe94-114">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="fbe94-115">Provedores personalizados (instalados ou criados)</span><span class="sxs-lookup"><span data-stu-id="fbe94-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="fbe94-116">Cada valor de configuração é mapeado para uma chave de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="fbe94-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="fbe94-117">Há suporte interno de associação para desserializar configurações em um objeto [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) personalizado (uma classe simples de .NET com propriedades).</span><span class="sxs-lookup"><span data-stu-id="fbe94-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="fbe94-118">O padrão de opções usa classes de opções para representar grupos de configurações relacionadas.</span><span class="sxs-lookup"><span data-stu-id="fbe94-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="fbe94-119">Para obter mais informações sobre como usar o padrão de opções, consulte o tópico [Opções](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="fbe94-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="fbe94-120">[Exibir ou baixar código de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fbe94-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="fbe94-121">Configuração de JSON</span><span class="sxs-lookup"><span data-stu-id="fbe94-121">JSON configuration</span></span>

<span data-ttu-id="fbe94-122">O aplicativo de console a seguir usa o provedor de configuração JSON:</span><span class="sxs-lookup"><span data-stu-id="fbe94-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="fbe94-123">O aplicativo lê e exibe as seguintes definições de configuração:</span><span class="sxs-lookup"><span data-stu-id="fbe94-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="fbe94-124">A configuração consiste em uma lista hierárquica de pares nome-valor na qual os nós são separados por dois pontos.</span><span class="sxs-lookup"><span data-stu-id="fbe94-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="fbe94-125">Para recuperar um valor, acesse o indexador `Configuration` com a chave do item correspondente:</span><span class="sxs-lookup"><span data-stu-id="fbe94-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="fbe94-126">Para trabalhar com matrizes em fontes de configuração formatadas em JSON, use um índice de matriz como parte da cadeia de caracteres separada por dois pontos.</span><span class="sxs-lookup"><span data-stu-id="fbe94-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="fbe94-127">O exemplo a seguir obtém o nome do primeiro item na matriz `wizards` anterior:</span><span class="sxs-lookup"><span data-stu-id="fbe94-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="fbe94-128">Os pares nome-valor gravados nos provedores de [Configuração](/dotnet/api/microsoft.extensions.configuration) internos **não** são mantidos.</span><span class="sxs-lookup"><span data-stu-id="fbe94-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="fbe94-129">No entanto, você pode criar um provedor personalizado que salva valores.</span><span class="sxs-lookup"><span data-stu-id="fbe94-129">However, you can create a custom provider that saves values.</span></span> <span data-ttu-id="fbe94-130">Consulte [provedor de configuração personalizado](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="fbe94-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="fbe94-131">O exemplo anterior usa o indexador de configuração para ler valores.</span><span class="sxs-lookup"><span data-stu-id="fbe94-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="fbe94-132">Para acessar a configuração fora de `Startup`, use o *padrão de opções*.</span><span class="sxs-lookup"><span data-stu-id="fbe94-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="fbe94-133">Para obter mais informações, consulte o tópico [Opções](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="fbe94-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>


## <a name="configuration-by-environment"></a><span data-ttu-id="fbe94-134">Configuração por ambiente</span><span class="sxs-lookup"><span data-stu-id="fbe94-134">Configuration by environment</span></span>

<span data-ttu-id="fbe94-135">É comum ter configurações diferentes para ambientes diferentes, por exemplo, desenvolvimento, teste e produção.</span><span class="sxs-lookup"><span data-stu-id="fbe94-135">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="fbe94-136">O método de extensão `CreateDefaultBuilder` em um aplicativo do ASP.NET Core 2.x (ou usando `AddJsonFile` e `AddEnvironmentVariables` diretamente em um aplicativo do ASP.NET Core 1.x) adiciona provedores de configuração para ler arquivos JSON e fontes de configuração do sistema:</span><span class="sxs-lookup"><span data-stu-id="fbe94-136">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="fbe94-137">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="fbe94-137">*appsettings.json*</span></span>
* <span data-ttu-id="fbe94-138">*appsettings.\<EnvironmentName>.json*</span><span class="sxs-lookup"><span data-stu-id="fbe94-138">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="fbe94-139">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="fbe94-139">Environment variables</span></span>

<span data-ttu-id="fbe94-140">Aplicativos do ASP.NET Core 1.x precisam chamar `AddJsonFile` e [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="fbe94-140">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="fbe94-141">Consulte [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) para uma explicação sobre os parâmetros.</span><span class="sxs-lookup"><span data-stu-id="fbe94-141">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="fbe94-142">`reloadOnChange` só é compatível no ASP.NET Core 1.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="fbe94-142">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="fbe94-143">As fontes de configuração são lidas na ordem em que são especificadas.</span><span class="sxs-lookup"><span data-stu-id="fbe94-143">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="fbe94-144">No código anterior, as variáveis de ambiente são lidas por último.</span><span class="sxs-lookup"><span data-stu-id="fbe94-144">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="fbe94-145">Os valores de configuração definidos por meio do ambiente substituem aqueles definidos nos dois provedores anteriores.</span><span class="sxs-lookup"><span data-stu-id="fbe94-145">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="fbe94-146">Considere o arquivo *appsettings.Staging.json* a seguir:</span><span class="sxs-lookup"><span data-stu-id="fbe94-146">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[Main](index/sample/appsettings.Staging.json)]

<span data-ttu-id="fbe94-147">Quando o ambiente está configurado como `Staging`, o seguinte método `Configure` lê o valor de `MyConfig`:</span><span class="sxs-lookup"><span data-stu-id="fbe94-147">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[Main](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="fbe94-148">O ambiente normalmente é definido como `Development`, `Staging` ou `Production`.</span><span class="sxs-lookup"><span data-stu-id="fbe94-148">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="fbe94-149">Para obter mais informações, consulte [Working with multiple environments](xref:fundamentals/environments) (Trabalhando com vários ambientes).</span><span class="sxs-lookup"><span data-stu-id="fbe94-149">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="fbe94-150">Considerações de configuração:</span><span class="sxs-lookup"><span data-stu-id="fbe94-150">Configuration considerations:</span></span>

* <span data-ttu-id="fbe94-151">`IOptionsSnapshot` pode recarregar dados de configuração quando é alterado.</span><span class="sxs-lookup"><span data-stu-id="fbe94-151">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="fbe94-152">Consulte [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="fbe94-152">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="fbe94-153">Chaves de configuração **não** diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="fbe94-153">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="fbe94-154">**Nunca** armazene senhas ou outros dados confidenciais no código do provedor de configuração ou nos arquivos de configuração de texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="fbe94-154">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="fbe94-155">Não use segredos de produção em seus ambientes de teste ou de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="fbe94-155">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="fbe94-156">Especifique segredos fora do projeto para que eles não sejam acidentalmente comprometidos com seu repositório.</span><span class="sxs-lookup"><span data-stu-id="fbe94-156">Specify secrets outside of the project so that they can't be accidentally committed to your repository.</span></span> <span data-ttu-id="fbe94-157">Saiba mais sobre [trabalhando com vários ambientes](xref:fundamentals/environments) e gerenciamento de [armazenamento seguro de segredos de aplicativo durante o desenvolvimento](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="fbe94-157">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="fbe94-158">Se dois pontos (`:`) não puder ser usado em variáveis de ambiente em seu sistema, substitua os dois pontos (`:`) por um sublinhado duplo (`__`).</span><span class="sxs-lookup"><span data-stu-id="fbe94-158">If a colon (`:`) can't be used in environment variables on your system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="fbe94-159">Provedor na memória e associação a uma classe POCO</span><span class="sxs-lookup"><span data-stu-id="fbe94-159">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="fbe94-160">O exemplo a seguir mostra como usar o provedor na memória e associar a uma classe:</span><span class="sxs-lookup"><span data-stu-id="fbe94-160">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="fbe94-161">Os valores de configuração são retornados como cadeias de caracteres, mas a associação permite a construção de objetos.</span><span class="sxs-lookup"><span data-stu-id="fbe94-161">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="fbe94-162">A associação permite que você recupere objetos POCO ou até mesmo gráficos de objetos inteiros.</span><span class="sxs-lookup"><span data-stu-id="fbe94-162">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="fbe94-163">GetValue</span><span class="sxs-lookup"><span data-stu-id="fbe94-163">GetValue</span></span>

<span data-ttu-id="fbe94-164">O exemplo a seguir demonstra o método de extensão [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_):</span><span class="sxs-lookup"><span data-stu-id="fbe94-164">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="fbe94-165">O método `GetValue<T>` do ConfigurationBinder permite que você especifique um valor padrão (80 no exemplo).</span><span class="sxs-lookup"><span data-stu-id="fbe94-165">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="fbe94-166">`GetValue<T>` é para cenários simples e não se associa a seções inteiras.</span><span class="sxs-lookup"><span data-stu-id="fbe94-166">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="fbe94-167">`GetValue<T>` obtém valores escalares de `GetSection(key).Value` convertidos em um tipo específico.</span><span class="sxs-lookup"><span data-stu-id="fbe94-167">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="fbe94-168">Associar a um gráfico de objeto</span><span class="sxs-lookup"><span data-stu-id="fbe94-168">Bind to an object graph</span></span>

<span data-ttu-id="fbe94-169">Você pode associar recursivamente a cada objeto em uma classe.</span><span class="sxs-lookup"><span data-stu-id="fbe94-169">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="fbe94-170">Considere a classe `AppSettings` a seguir:</span><span class="sxs-lookup"><span data-stu-id="fbe94-170">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="fbe94-171">O exemplo a seguir associa à classe `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="fbe94-171">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="fbe94-172">O **ASP.NET Core 1.1** e superior pode usar `Get<T>`, que trabalha com seções inteiras.</span><span class="sxs-lookup"><span data-stu-id="fbe94-172">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="fbe94-173">O `Get<T>` pode ser mais conveniente do que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="fbe94-173">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="fbe94-174">O código a seguir mostra como usar o `Get<T>` com o exemplo acima:</span><span class="sxs-lookup"><span data-stu-id="fbe94-174">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="fbe94-175">Usando o seguinte arquivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="fbe94-175">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="fbe94-176">O programa exibe `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="fbe94-176">The program displays `Height 11`.</span></span>

<span data-ttu-id="fbe94-177">O código a seguir pode ser usado para o teste de unidade da configuração:</span><span class="sxs-lookup"><span data-stu-id="fbe94-177">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="fbe94-178">Criar um provedor personalizado do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="fbe94-178">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="fbe94-179">Nesta seção, é criado um provedor de configuração básico, que lê os pares nome-valor de um banco de dados, usando o EF.</span><span class="sxs-lookup"><span data-stu-id="fbe94-179">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="fbe94-180">Defina uma entidade `ConfigurationValue` para armazenar valores de configuração no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="fbe94-180">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="fbe94-181">Adicione um `ConfigurationContext` para armazenar e acessar os valores configurados:</span><span class="sxs-lookup"><span data-stu-id="fbe94-181">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="fbe94-182">Crie uma classe que implementa [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span><span class="sxs-lookup"><span data-stu-id="fbe94-182">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="fbe94-183">Crie o provedor de configuração personalizado através da herança do [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="fbe94-183">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="fbe94-184">O provedor de configuração inicializa o banco de dados quando ele está vazio:</span><span class="sxs-lookup"><span data-stu-id="fbe94-184">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="fbe94-185">Os valores realçados do banco de dados ("value_from_ef_1" e "value_from_ef_2") são exibidos quando o exemplo é executado.</span><span class="sxs-lookup"><span data-stu-id="fbe94-185">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="fbe94-186">Você pode adicionar um método de extensão `EFConfigSource` para adicionar a fonte de configuração:</span><span class="sxs-lookup"><span data-stu-id="fbe94-186">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="fbe94-187">O código a seguir mostra como usar o `EFConfigProvider` personalizado:</span><span class="sxs-lookup"><span data-stu-id="fbe94-187">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="fbe94-188">Observe que o exemplo adiciona o `EFConfigProvider` personalizado depois do provedor JSON, de maneira que todas as configurações do banco de dados substituam as configurações do arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="fbe94-188">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="fbe94-189">Usando o seguinte arquivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="fbe94-189">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="fbe94-190">É exibida a saída a seguir:</span><span class="sxs-lookup"><span data-stu-id="fbe94-190">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="fbe94-191">Provedor de configuração CommandLine</span><span class="sxs-lookup"><span data-stu-id="fbe94-191">CommandLine configuration provider</span></span>

<span data-ttu-id="fbe94-192">O [Provedor de configuração CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) recebe pares chave-valor de argumento de linha de comando para a configuração em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="fbe94-192">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="fbe94-193">Exibir ou baixar o exemplo de configuração do CommandLine</span><span class="sxs-lookup"><span data-stu-id="fbe94-193">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="fbe94-194">Configurar e usar o provedor de configuração CommandLine</span><span class="sxs-lookup"><span data-stu-id="fbe94-194">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="fbe94-195">Configuração básica</span><span class="sxs-lookup"><span data-stu-id="fbe94-195">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="fbe94-196">Para ativar a configuração de linha de comando, chame o método de extensão `AddCommandLine` em uma instância do [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="fbe94-196">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="fbe94-197">Ao executar o código, a seguinte saída será exibida:</span><span class="sxs-lookup"><span data-stu-id="fbe94-197">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="fbe94-198">Passar pares chave-valor de argumento na linha de comando altera os valores de `Profile:MachineName` e `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="fbe94-198">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="fbe94-199">A janela do console exibe:</span><span class="sxs-lookup"><span data-stu-id="fbe94-199">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="fbe94-200">Para substituir a configuração fornecida por outros provedores de configuração pela configuração de linha de comando, chame `AddCommandLine` por último em `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="fbe94-200">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="fbe94-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fbe94-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="fbe94-202">Os aplicativos típicos de ASP.NET Core 2.x usam o método de conveniência estático `CreateDefaultBuilder` para criar o host:</span><span class="sxs-lookup"><span data-stu-id="fbe94-202">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="fbe94-203">O `CreateDefaultBuilder` carrega a configuração opcional de *appsettings.json*, de *appsettings.{Environment}.json*, dos [segredos do usuário](xref:security/app-secrets) (no ambiente `Development`), das variáveis de ambiente e dos argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="fbe94-203">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="fbe94-204">O provedor CommandLine é chamado por último.</span><span class="sxs-lookup"><span data-stu-id="fbe94-204">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="fbe94-205">Chamar o provedor por último permite que os argumentos de linha de comando passados em tempo de execução substituam a configuração definida por outros provedores de configuração chamados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="fbe94-205">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="fbe94-206">Para arquivos *appsettings* em que:</span><span class="sxs-lookup"><span data-stu-id="fbe94-206">For *appsettings* files where:</span></span>

* <span data-ttu-id="fbe94-207">`reloadOnChange` está habilitado.</span><span class="sxs-lookup"><span data-stu-id="fbe94-207">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="fbe94-208">Contém a mesma configuração nos argumentos de linha de comando e um arquivo *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="fbe94-208">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="fbe94-209">O arquivo *appsettings* que contém o argumento de linha de comando correspondente é alterado depois que o aplicativo for iniciado.</span><span class="sxs-lookup"><span data-stu-id="fbe94-209">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="fbe94-210">Se todas as condições acima forem verdadeiras, os argumentos de linha de comando serão substituídos.</span><span class="sxs-lookup"><span data-stu-id="fbe94-210">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="fbe94-211">O aplicativo do ASP.NET Core 2.x pode usar WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) em vez de \`\`CreateDefaultBuilder`. When using `WebHostBuilder\`, defina a configuração manualmente com [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="fbe94-211">ASP.NET Core 2.x app can use WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of \`\`CreateDefaultBuilder`. When using `WebHostBuilder\`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="fbe94-212">Consulte a guia do ASP.NET Core 1.x para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="fbe94-212">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="fbe94-213">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="fbe94-213">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="fbe94-214">Crie um [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) e chame o método `AddCommandLine` para usar o provedor de configuração CommandLine.</span><span class="sxs-lookup"><span data-stu-id="fbe94-214">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="fbe94-215">Chamar o provedor por último permite que os argumentos de linha de comando passados em tempo de execução substituam a configuração definida por outros provedores de configuração chamados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="fbe94-215">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="fbe94-216">Aplique a configuração ao [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) com o método `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="fbe94-216">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="fbe94-217">Arguments</span><span class="sxs-lookup"><span data-stu-id="fbe94-217">Arguments</span></span>

<span data-ttu-id="fbe94-218">Os argumentos passados na linha de comando devem estar em conformidade com um dos dois formatos mostrados na tabela a seguir:</span><span class="sxs-lookup"><span data-stu-id="fbe94-218">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="fbe94-219">Formato de argumento</span><span class="sxs-lookup"><span data-stu-id="fbe94-219">Argument format</span></span>                                                     | <span data-ttu-id="fbe94-220">Exemplo</span><span class="sxs-lookup"><span data-stu-id="fbe94-220">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="fbe94-221">Argumento único: um par chave-valor separado por um sinal de igual (`=`)</span><span class="sxs-lookup"><span data-stu-id="fbe94-221">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="fbe94-222">Sequência de dois argumentos: um par chave-valor separado por um espaço</span><span class="sxs-lookup"><span data-stu-id="fbe94-222">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="fbe94-223">**Argumento único**</span><span class="sxs-lookup"><span data-stu-id="fbe94-223">**Single argument**</span></span>

<span data-ttu-id="fbe94-224">O valor deve seguir um sinal de igual (`=`).</span><span class="sxs-lookup"><span data-stu-id="fbe94-224">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="fbe94-225">O valor pode ser nulo (por exemplo, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="fbe94-225">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="fbe94-226">A chave pode ter um prefixo.</span><span class="sxs-lookup"><span data-stu-id="fbe94-226">The key may have a prefix.</span></span>

| <span data-ttu-id="fbe94-227">Prefixo da chave</span><span class="sxs-lookup"><span data-stu-id="fbe94-227">Key prefix</span></span>               | <span data-ttu-id="fbe94-228">Exemplo</span><span class="sxs-lookup"><span data-stu-id="fbe94-228">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="fbe94-229">Nenhum prefixo</span><span class="sxs-lookup"><span data-stu-id="fbe94-229">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="fbe94-230">Traço único (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="fbe94-230">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="fbe94-231">Dois traços (`--`)</span><span class="sxs-lookup"><span data-stu-id="fbe94-231">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="fbe94-232">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="fbe94-232">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="fbe94-233">&#8224;Uma chave com um prefixo de traço único (`-`) deve ser fornecida em [mapeamentos de comutador](#switch-mappings), como descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="fbe94-233">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="fbe94-234">Comando de exemplo:</span><span class="sxs-lookup"><span data-stu-id="fbe94-234">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="fbe94-235">Observação: se `-key1` não estiver presente nos [mapeamentos de comutador](#switch-mappings) fornecidos para o provedor de configuração, uma `FormatException` será gerada.</span><span class="sxs-lookup"><span data-stu-id="fbe94-235">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="fbe94-236">**Sequência de dois argumentos**</span><span class="sxs-lookup"><span data-stu-id="fbe94-236">**Sequence of two arguments**</span></span>

<span data-ttu-id="fbe94-237">O valor não pode ser nulo e deve seguir a chave separada por um espaço.</span><span class="sxs-lookup"><span data-stu-id="fbe94-237">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="fbe94-238">A chave deve ter um prefixo.</span><span class="sxs-lookup"><span data-stu-id="fbe94-238">The key must have a prefix.</span></span>

| <span data-ttu-id="fbe94-239">Prefixo da chave</span><span class="sxs-lookup"><span data-stu-id="fbe94-239">Key prefix</span></span>               | <span data-ttu-id="fbe94-240">Exemplo</span><span class="sxs-lookup"><span data-stu-id="fbe94-240">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="fbe94-241">Traço único (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="fbe94-241">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="fbe94-242">Dois traços (`--`)</span><span class="sxs-lookup"><span data-stu-id="fbe94-242">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="fbe94-243">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="fbe94-243">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="fbe94-244">&#8224;Uma chave com um prefixo de traço único (`-`) deve ser fornecida em [mapeamentos de comutador](#switch-mappings), como descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="fbe94-244">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="fbe94-245">Comando de exemplo:</span><span class="sxs-lookup"><span data-stu-id="fbe94-245">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="fbe94-246">Observação: se `-key1` não estiver presente nos [mapeamentos de comutador](#switch-mappings) fornecidos para o provedor de configuração, uma `FormatException` será gerada.</span><span class="sxs-lookup"><span data-stu-id="fbe94-246">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="fbe94-247">Chaves duplicadas</span><span class="sxs-lookup"><span data-stu-id="fbe94-247">Duplicate keys</span></span>

<span data-ttu-id="fbe94-248">Se chaves duplicadas forem fornecidas, o último par chave-valor será usado.</span><span class="sxs-lookup"><span data-stu-id="fbe94-248">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="fbe94-249">Mapeamentos de comutador</span><span class="sxs-lookup"><span data-stu-id="fbe94-249">Switch mappings</span></span>

<span data-ttu-id="fbe94-250">Ao criar manualmente a configuração com `ConfigurationBuilder`, você pode fornecer opcionalmente um dicionário de mapeamentos de comutador para o método `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="fbe94-250">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="fbe94-251">Os mapeamentos de comutador permitem que você forneça a lógica de substituição do nome da chave.</span><span class="sxs-lookup"><span data-stu-id="fbe94-251">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="fbe94-252">Ao ser usado, o dicionário de mapeamentos de comutador é verificado para oferecer uma chave que corresponda à chave fornecida por um argumento de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="fbe94-252">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="fbe94-253">Se a chave de linha de comando for encontrada no dicionário, o valor do dicionário (a substituição da chave) será passado de volta para definir a configuração.</span><span class="sxs-lookup"><span data-stu-id="fbe94-253">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="fbe94-254">Um mapeamento de comutador é necessário para qualquer chave de linha de comando prefixada com um traço único (`-`).</span><span class="sxs-lookup"><span data-stu-id="fbe94-254">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="fbe94-255">Regras de chave do dicionário de mapeamentos de comutador:</span><span class="sxs-lookup"><span data-stu-id="fbe94-255">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="fbe94-256">Os comutadores devem começar com um traço (`-`) ou traço duplo (`--`).</span><span class="sxs-lookup"><span data-stu-id="fbe94-256">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="fbe94-257">O dicionário de mapeamentos de comutador chave não deve conter chaves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="fbe94-257">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="fbe94-258">No exemplo a seguir, o método `GetSwitchMappings` permite que seus argumentos de linha de comando usem um prefixo de chave de traço único (`-`) e evitem prefixos de subchaves à esquerda.</span><span class="sxs-lookup"><span data-stu-id="fbe94-258">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="fbe94-259">Sem fornecer argumentos de linha de comando, o dicionário fornecido para `AddInMemoryCollection` definirá os valores de configuração.</span><span class="sxs-lookup"><span data-stu-id="fbe94-259">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="fbe94-260">Execute o aplicativo com o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="fbe94-260">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="fbe94-261">A janela do console exibe:</span><span class="sxs-lookup"><span data-stu-id="fbe94-261">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="fbe94-262">Use o seguinte para passar parâmetros de configuração:</span><span class="sxs-lookup"><span data-stu-id="fbe94-262">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="fbe94-263">A janela do console exibe:</span><span class="sxs-lookup"><span data-stu-id="fbe94-263">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="fbe94-264">Depois que o dicionário de mapeamentos de comutador for criado, ele conterá os dados mostrados na tabela a seguir:</span><span class="sxs-lookup"><span data-stu-id="fbe94-264">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="fbe94-265">Chave</span><span class="sxs-lookup"><span data-stu-id="fbe94-265">Key</span></span>            | <span data-ttu-id="fbe94-266">Valor</span><span class="sxs-lookup"><span data-stu-id="fbe94-266">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="fbe94-267">Para demonstrar a troca de chaves usando o dicionário, execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="fbe94-267">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="fbe94-268">As chaves da linha de comando são trocadas.</span><span class="sxs-lookup"><span data-stu-id="fbe94-268">The command-line keys are swapped.</span></span> <span data-ttu-id="fbe94-269">A janela de console exibe os valores de configuração de `Profile:MachineName` e `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="fbe94-269">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="fbe94-270">O arquivo Web.config</span><span class="sxs-lookup"><span data-stu-id="fbe94-270">The web.config file</span></span>

<span data-ttu-id="fbe94-271">Um arquivo *web.config* é necessário quando você hospeda o aplicativo em IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="fbe94-271">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="fbe94-272">As configurações no *web.config* habilitam o [Módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para iniciar o aplicativo e definir outras configurações e módulos do IIS.</span><span class="sxs-lookup"><span data-stu-id="fbe94-272">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="fbe94-273">Se o arquivo *web.config* não estiver presente e o arquivo de projeto inclui `<Project Sdk="Microsoft.NET.Sdk.Web">`, a publicação do projeto criará um arquivo *web.config* na saída publicada (a pasta *publish*).</span><span class="sxs-lookup"><span data-stu-id="fbe94-273">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="fbe94-274">Para obter mais informações, consulte [Hospedar o ASP.NET Core no Windows com o IIS](xref:host-and-deploy/iis/index#webconfig).</span><span class="sxs-lookup"><span data-stu-id="fbe94-274">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig).</span></span>

## <a name="additional-notes"></a><span data-ttu-id="fbe94-275">Observações adicionais</span><span class="sxs-lookup"><span data-stu-id="fbe94-275">Additional notes</span></span>

* <span data-ttu-id="fbe94-276">A DI (injeção de dependência) não é configurada até que `ConfigureServices` seja invocado.</span><span class="sxs-lookup"><span data-stu-id="fbe94-276">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="fbe94-277">O sistema de configuração não tem reconhecimento de DI.</span><span class="sxs-lookup"><span data-stu-id="fbe94-277">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="fbe94-278">O `IConfiguration` tem duas especializações:</span><span class="sxs-lookup"><span data-stu-id="fbe94-278">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="fbe94-279">`IConfigurationRoot` Usado para o nó raiz.</span><span class="sxs-lookup"><span data-stu-id="fbe94-279">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="fbe94-280">Pode disparar um recarregamento.</span><span class="sxs-lookup"><span data-stu-id="fbe94-280">Can trigger a reload.</span></span>
  * <span data-ttu-id="fbe94-281">`IConfigurationSection` Representa uma seção de valores de configuração.</span><span class="sxs-lookup"><span data-stu-id="fbe94-281">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="fbe94-282">O métodos `GetSection` e `GetChildren` retornam um `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="fbe94-282">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="fbe94-283">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) ao recarregar a configuração ou se precisar de acesso para cada provedor.</span><span class="sxs-lookup"><span data-stu-id="fbe94-283">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or need access to each provider.</span></span> <span data-ttu-id="fbe94-284">Nenhuma dessas situações são comuns.</span><span class="sxs-lookup"><span data-stu-id="fbe94-284">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fbe94-285">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="fbe94-285">Additional resources</span></span>

* [<span data-ttu-id="fbe94-286">Opções</span><span class="sxs-lookup"><span data-stu-id="fbe94-286">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="fbe94-287">Trabalhando com vários ambientes</span><span class="sxs-lookup"><span data-stu-id="fbe94-287">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="fbe94-288">Armazenamento seguro dos segredos do aplicativo durante o desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="fbe94-288">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="fbe94-289">Hospedagem no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fbe94-289">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="fbe94-290">Injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="fbe94-290">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="fbe94-291">Provedor de configuração do Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="fbe94-291">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)

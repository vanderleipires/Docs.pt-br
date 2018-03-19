---
title: "Configuração no ASP.NET Core"
author: rick-anderson
description: "Use a API de configuração para configurar um aplicativo do ASP.NET Core por vários métodos."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: 7c41621db835b452c9aad9463a9ffccdf0c06484
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="038ff-103">Configurar um aplicativo do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="038ff-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="038ff-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="038ff-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="038ff-105">A API de configuração fornece uma maneira de configurar um aplicativo Web do ASP.NET Core com base em uma lista de pares nome-valor.</span><span class="sxs-lookup"><span data-stu-id="038ff-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="038ff-106">A configuração é lida em tempo de execução por meio de várias fontes.</span><span class="sxs-lookup"><span data-stu-id="038ff-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="038ff-107">Os pares de nome-valor podem ser agrupados em uma hierarquia de vários níveis.</span><span class="sxs-lookup"><span data-stu-id="038ff-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="038ff-108">Há provedores de configuração para:</span><span class="sxs-lookup"><span data-stu-id="038ff-108">There are configuration providers for:</span></span>

* <span data-ttu-id="038ff-109">Formatos de arquivo (INI, JSON e XML).</span><span class="sxs-lookup"><span data-stu-id="038ff-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="038ff-110">Argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="038ff-110">Command-line arguments.</span></span>
* <span data-ttu-id="038ff-111">Variáveis de ambiente.</span><span class="sxs-lookup"><span data-stu-id="038ff-111">Environment variables.</span></span>
* <span data-ttu-id="038ff-112">Objetos do .NET na memória.</span><span class="sxs-lookup"><span data-stu-id="038ff-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="038ff-113">O armazenamento do [Secret Manager](xref:security/app-secrets) não criptografado.</span><span class="sxs-lookup"><span data-stu-id="038ff-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="038ff-114">Um repositório de usuário criptografado, como o [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="038ff-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="038ff-115">Provedores personalizados (instalados ou criados).</span><span class="sxs-lookup"><span data-stu-id="038ff-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="038ff-116">Cada valor de configuração é mapeado para uma chave de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="038ff-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="038ff-117">Há suporte interno de associação para desserializar configurações em um objeto [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) personalizado (uma classe simples de .NET com propriedades).</span><span class="sxs-lookup"><span data-stu-id="038ff-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="038ff-118">O padrão de opções usa classes de opções para representar grupos de configurações relacionadas.</span><span class="sxs-lookup"><span data-stu-id="038ff-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="038ff-119">Para obter mais informações sobre como usar o padrão de opções, consulte o tópico [Opções](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="038ff-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="038ff-120">[Exibir ou baixar código de exemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="038ff-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="038ff-121">Configuração de JSON</span><span class="sxs-lookup"><span data-stu-id="038ff-121">JSON configuration</span></span>

<span data-ttu-id="038ff-122">O aplicativo de console a seguir usa o provedor de configuração JSON:</span><span class="sxs-lookup"><span data-stu-id="038ff-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="038ff-123">O aplicativo lê e exibe as seguintes definições de configuração:</span><span class="sxs-lookup"><span data-stu-id="038ff-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="038ff-124">A configuração consiste em uma lista hierárquica de pares nome-valor na qual os nós são separados por dois pontos.</span><span class="sxs-lookup"><span data-stu-id="038ff-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="038ff-125">Para recuperar um valor, acesse o indexador `Configuration` com a chave do item correspondente:</span><span class="sxs-lookup"><span data-stu-id="038ff-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="038ff-126">Para trabalhar com matrizes em fontes de configuração formatadas em JSON, use um índice de matriz como parte da cadeia de caracteres separada por dois pontos.</span><span class="sxs-lookup"><span data-stu-id="038ff-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="038ff-127">O exemplo a seguir obtém o nome do primeiro item na matriz `wizards` anterior:</span><span class="sxs-lookup"><span data-stu-id="038ff-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="038ff-128">Os pares nome-valor gravados nos provedores de [Configuração](/dotnet/api/microsoft.extensions.configuration) internos **não** são mantidos.</span><span class="sxs-lookup"><span data-stu-id="038ff-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="038ff-129">No entanto, um provedor personalizado que salva valores pode ser criado.</span><span class="sxs-lookup"><span data-stu-id="038ff-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="038ff-130">Consulte [provedor de configuração personalizado](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="038ff-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="038ff-131">O exemplo anterior usa o indexador de configuração para ler valores.</span><span class="sxs-lookup"><span data-stu-id="038ff-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="038ff-132">Para acessar a configuração fora de `Startup`, use o *padrão de opções*.</span><span class="sxs-lookup"><span data-stu-id="038ff-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="038ff-133">Para obter mais informações, consulte o tópico [Opções](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="038ff-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="038ff-134">Configuração XML</span><span class="sxs-lookup"><span data-stu-id="038ff-134">XML configuration</span></span>

<span data-ttu-id="038ff-135">Para trabalhar com matrizes em fontes de configuração formatadas em XML, forneça um índice `name` para cada elemento.</span><span class="sxs-lookup"><span data-stu-id="038ff-135">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="038ff-136">Use o índice para acessar os valores:</span><span class="sxs-lookup"><span data-stu-id="038ff-136">Use the index to access the values:</span></span>

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a><span data-ttu-id="038ff-137">Configuração por ambiente</span><span class="sxs-lookup"><span data-stu-id="038ff-137">Configuration by environment</span></span>

<span data-ttu-id="038ff-138">É comum ter configurações diferentes para ambientes diferentes, por exemplo, desenvolvimento, teste e produção.</span><span class="sxs-lookup"><span data-stu-id="038ff-138">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="038ff-139">O método de extensão `CreateDefaultBuilder` em um aplicativo do ASP.NET Core 2.x (ou usando `AddJsonFile` e `AddEnvironmentVariables` diretamente em um aplicativo do ASP.NET Core 1.x) adiciona provedores de configuração para ler arquivos JSON e fontes de configuração do sistema:</span><span class="sxs-lookup"><span data-stu-id="038ff-139">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="038ff-140">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="038ff-140">*appsettings.json*</span></span>
* <span data-ttu-id="038ff-141">*appsettings.\<EnvironmentName>.json*</span><span class="sxs-lookup"><span data-stu-id="038ff-141">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="038ff-142">Variáveis de ambiente</span><span class="sxs-lookup"><span data-stu-id="038ff-142">Environment variables</span></span>

<span data-ttu-id="038ff-143">Aplicativos do ASP.NET Core 1.x precisam chamar `AddJsonFile` e [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="038ff-143">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="038ff-144">Consulte [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) para uma explicação sobre os parâmetros.</span><span class="sxs-lookup"><span data-stu-id="038ff-144">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="038ff-145">`reloadOnChange` só é compatível no ASP.NET Core 1.1 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="038ff-145">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="038ff-146">As fontes de configuração são lidas na ordem em que são especificadas.</span><span class="sxs-lookup"><span data-stu-id="038ff-146">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="038ff-147">No código anterior, as variáveis de ambiente são lidas por último.</span><span class="sxs-lookup"><span data-stu-id="038ff-147">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="038ff-148">Os valores de configuração definidos por meio do ambiente substituem aqueles definidos nos dois provedores anteriores.</span><span class="sxs-lookup"><span data-stu-id="038ff-148">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="038ff-149">Considere o arquivo *appsettings.Staging.json* a seguir:</span><span class="sxs-lookup"><span data-stu-id="038ff-149">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="038ff-150">Quando o ambiente está configurado como `Staging`, o seguinte método `Configure` lê o valor de `MyConfig`:</span><span class="sxs-lookup"><span data-stu-id="038ff-150">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="038ff-151">O ambiente normalmente é definido como `Development`, `Staging` ou `Production`.</span><span class="sxs-lookup"><span data-stu-id="038ff-151">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="038ff-152">Para obter mais informações, consulte [Working with multiple environments](xref:fundamentals/environments) (Trabalhando com vários ambientes).</span><span class="sxs-lookup"><span data-stu-id="038ff-152">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="038ff-153">Considerações de configuração:</span><span class="sxs-lookup"><span data-stu-id="038ff-153">Configuration considerations:</span></span>

* <span data-ttu-id="038ff-154">`IOptionsSnapshot` pode recarregar dados de configuração quando é alterado.</span><span class="sxs-lookup"><span data-stu-id="038ff-154">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="038ff-155">Consulte [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="038ff-155">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="038ff-156">Chaves de configuração **não** diferenciam maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="038ff-156">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="038ff-157">**Nunca** armazene senhas ou outros dados confidenciais no código do provedor de configuração ou nos arquivos de configuração de texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="038ff-157">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="038ff-158">Não use segredos de produção em ambientes de teste ou de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="038ff-158">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="038ff-159">Especifique segredos fora do projeto para que eles não sejam acidentalmente comprometidos com um repositório de código-fonte.</span><span class="sxs-lookup"><span data-stu-id="038ff-159">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="038ff-160">Saiba mais sobre [trabalhando com vários ambientes](xref:fundamentals/environments) e gerenciamento de [armazenamento seguro de segredos de aplicativo durante o desenvolvimento](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="038ff-160">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="038ff-161">Se não for possível usar dois-pontos (`:`) em variáveis de ambiente em um sistema, substitua os dois-pontos (`:`) por um sublinhado duplo (`__`).</span><span class="sxs-lookup"><span data-stu-id="038ff-161">If a colon (`:`) can't be used in environment variables on a system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="038ff-162">Provedor na memória e associação a uma classe POCO</span><span class="sxs-lookup"><span data-stu-id="038ff-162">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="038ff-163">O exemplo a seguir mostra como usar o provedor na memória e associar a uma classe:</span><span class="sxs-lookup"><span data-stu-id="038ff-163">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="038ff-164">Os valores de configuração são retornados como cadeias de caracteres, mas a associação permite a construção de objetos.</span><span class="sxs-lookup"><span data-stu-id="038ff-164">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="038ff-165">A associação permite recuperar objetos POCO ou até mesmo gráficos de objetos inteiros.</span><span class="sxs-lookup"><span data-stu-id="038ff-165">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="038ff-166">GetValue</span><span class="sxs-lookup"><span data-stu-id="038ff-166">GetValue</span></span>

<span data-ttu-id="038ff-167">O exemplo a seguir demonstra o método de extensão [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_):</span><span class="sxs-lookup"><span data-stu-id="038ff-167">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="038ff-168">O método `GetValue<T>` do ConfigurationBinder permite especificar um valor padrão (80 no exemplo).</span><span class="sxs-lookup"><span data-stu-id="038ff-168">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="038ff-169">`GetValue<T>` é para cenários simples e não se associa a seções inteiras.</span><span class="sxs-lookup"><span data-stu-id="038ff-169">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="038ff-170">`GetValue<T>` obtém valores escalares de `GetSection(key).Value` convertidos em um tipo específico.</span><span class="sxs-lookup"><span data-stu-id="038ff-170">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="038ff-171">Associar a um gráfico de objeto</span><span class="sxs-lookup"><span data-stu-id="038ff-171">Bind to an object graph</span></span>

<span data-ttu-id="038ff-172">Cada objeto em uma classe pode ser associado recursivamente.</span><span class="sxs-lookup"><span data-stu-id="038ff-172">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="038ff-173">Considere a classe `AppSettings` a seguir:</span><span class="sxs-lookup"><span data-stu-id="038ff-173">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="038ff-174">O exemplo a seguir associa à classe `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="038ff-174">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="038ff-175">O **ASP.NET Core 1.1** e superior pode usar `Get<T>`, que trabalha com seções inteiras.</span><span class="sxs-lookup"><span data-stu-id="038ff-175">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="038ff-176">O `Get<T>` pode ser mais conveniente do que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="038ff-176">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="038ff-177">O código a seguir mostra como usar o `Get<T>` com o exemplo acima:</span><span class="sxs-lookup"><span data-stu-id="038ff-177">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="038ff-178">Usando o seguinte arquivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="038ff-178">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="038ff-179">O programa exibe `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="038ff-179">The program displays `Height 11`.</span></span>

<span data-ttu-id="038ff-180">O código a seguir pode ser usado para o teste de unidade da configuração:</span><span class="sxs-lookup"><span data-stu-id="038ff-180">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="038ff-181">Criar um provedor personalizado do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="038ff-181">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="038ff-182">Nesta seção, é criado um provedor de configuração básico, que lê os pares nome-valor de um banco de dados, usando o EF.</span><span class="sxs-lookup"><span data-stu-id="038ff-182">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="038ff-183">Defina uma entidade `ConfigurationValue` para armazenar valores de configuração no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="038ff-183">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="038ff-184">Adicione um `ConfigurationContext` para armazenar e acessar os valores configurados:</span><span class="sxs-lookup"><span data-stu-id="038ff-184">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="038ff-185">Crie uma classe que implementa [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span><span class="sxs-lookup"><span data-stu-id="038ff-185">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="038ff-186">Crie o provedor de configuração personalizado através da herança do [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="038ff-186">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="038ff-187">O provedor de configuração inicializa o banco de dados quando ele está vazio:</span><span class="sxs-lookup"><span data-stu-id="038ff-187">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="038ff-188">Os valores realçados do banco de dados ("value_from_ef_1" e "value_from_ef_2") são exibidos quando o exemplo é executado.</span><span class="sxs-lookup"><span data-stu-id="038ff-188">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="038ff-189">Um método de extensão `EFConfigSource` para adicionar a fonte de configuração pode ser usado:</span><span class="sxs-lookup"><span data-stu-id="038ff-189">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="038ff-190">O código a seguir mostra como usar o `EFConfigProvider` personalizado:</span><span class="sxs-lookup"><span data-stu-id="038ff-190">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="038ff-191">Observe que o exemplo adiciona o `EFConfigProvider` personalizado depois do provedor JSON, de maneira que todas as configurações do banco de dados substituam as configurações do arquivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="038ff-191">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="038ff-192">Usando o seguinte arquivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="038ff-192">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="038ff-193">É exibida a saída a seguir:</span><span class="sxs-lookup"><span data-stu-id="038ff-193">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="038ff-194">Provedor de configuração CommandLine</span><span class="sxs-lookup"><span data-stu-id="038ff-194">CommandLine configuration provider</span></span>

<span data-ttu-id="038ff-195">O [Provedor de configuração CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) recebe pares chave-valor de argumento de linha de comando para a configuração em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="038ff-195">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="038ff-196">Exibir ou baixar o exemplo de configuração do CommandLine</span><span class="sxs-lookup"><span data-stu-id="038ff-196">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="038ff-197">Configurar e usar o provedor de configuração CommandLine</span><span class="sxs-lookup"><span data-stu-id="038ff-197">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="038ff-198">Configuração básica</span><span class="sxs-lookup"><span data-stu-id="038ff-198">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="038ff-199">Para ativar a configuração de linha de comando, chame o método de extensão `AddCommandLine` em uma instância do [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="038ff-199">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="038ff-200">Ao executar o código, a seguinte saída será exibida:</span><span class="sxs-lookup"><span data-stu-id="038ff-200">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="038ff-201">Passar pares chave-valor de argumento na linha de comando altera os valores de `Profile:MachineName` e `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="038ff-201">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="038ff-202">A janela do console exibe:</span><span class="sxs-lookup"><span data-stu-id="038ff-202">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="038ff-203">Para substituir a configuração fornecida por outros provedores de configuração pela configuração de linha de comando, chame `AddCommandLine` por último em `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="038ff-203">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="038ff-204">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="038ff-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="038ff-205">Os aplicativos típicos de ASP.NET Core 2.x usam o método de conveniência estático `CreateDefaultBuilder` para criar o host:</span><span class="sxs-lookup"><span data-stu-id="038ff-205">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="038ff-206">O `CreateDefaultBuilder` carrega a configuração opcional de *appsettings.json*, de *appsettings.{Environment}.json*, dos [segredos do usuário](xref:security/app-secrets) (no ambiente `Development`), das variáveis de ambiente e dos argumentos de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="038ff-206">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="038ff-207">O provedor CommandLine é chamado por último.</span><span class="sxs-lookup"><span data-stu-id="038ff-207">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="038ff-208">Chamar o provedor por último permite que os argumentos de linha de comando passados em tempo de execução substituam a configuração definida por outros provedores de configuração chamados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="038ff-208">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="038ff-209">Para arquivos *appsettings* em que:</span><span class="sxs-lookup"><span data-stu-id="038ff-209">For *appsettings* files where:</span></span>

* <span data-ttu-id="038ff-210">`reloadOnChange` está habilitado.</span><span class="sxs-lookup"><span data-stu-id="038ff-210">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="038ff-211">Contém a mesma configuração nos argumentos de linha de comando e um arquivo *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="038ff-211">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="038ff-212">O arquivo *appsettings* que contém o argumento de linha de comando correspondente é alterado depois que o aplicativo for iniciado.</span><span class="sxs-lookup"><span data-stu-id="038ff-212">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="038ff-213">Se todas as condições acima forem verdadeiras, os argumentos de linha de comando serão substituídos.</span><span class="sxs-lookup"><span data-stu-id="038ff-213">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="038ff-214">O aplicativo do ASP.NET Core 2.x pode usar [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) em vez de `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="038ff-214">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="038ff-215">Ao usar `WebHostBuilder`, defina manualmente a configuração com [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="038ff-215">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="038ff-216">Consulte a guia do ASP.NET Core 1.x para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="038ff-216">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="038ff-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="038ff-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="038ff-218">Crie um [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) e chame o método `AddCommandLine` para usar o provedor de configuração CommandLine.</span><span class="sxs-lookup"><span data-stu-id="038ff-218">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="038ff-219">Chamar o provedor por último permite que os argumentos de linha de comando passados em tempo de execução substituam a configuração definida por outros provedores de configuração chamados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="038ff-219">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="038ff-220">Aplique a configuração ao [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) com o método `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="038ff-220">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="038ff-221">Arguments</span><span class="sxs-lookup"><span data-stu-id="038ff-221">Arguments</span></span>

<span data-ttu-id="038ff-222">Os argumentos passados na linha de comando devem estar em conformidade com um dos dois formatos mostrados na tabela a seguir:</span><span class="sxs-lookup"><span data-stu-id="038ff-222">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="038ff-223">Formato de argumento</span><span class="sxs-lookup"><span data-stu-id="038ff-223">Argument format</span></span>                                                     | <span data-ttu-id="038ff-224">Exemplo</span><span class="sxs-lookup"><span data-stu-id="038ff-224">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="038ff-225">Argumento único: um par chave-valor separado por um sinal de igual (`=`)</span><span class="sxs-lookup"><span data-stu-id="038ff-225">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="038ff-226">Sequência de dois argumentos: um par chave-valor separado por um espaço</span><span class="sxs-lookup"><span data-stu-id="038ff-226">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="038ff-227">**Argumento único**</span><span class="sxs-lookup"><span data-stu-id="038ff-227">**Single argument**</span></span>

<span data-ttu-id="038ff-228">O valor deve seguir um sinal de igual (`=`).</span><span class="sxs-lookup"><span data-stu-id="038ff-228">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="038ff-229">O valor pode ser nulo (por exemplo, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="038ff-229">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="038ff-230">A chave pode ter um prefixo.</span><span class="sxs-lookup"><span data-stu-id="038ff-230">The key may have a prefix.</span></span>

| <span data-ttu-id="038ff-231">Prefixo da chave</span><span class="sxs-lookup"><span data-stu-id="038ff-231">Key prefix</span></span>               | <span data-ttu-id="038ff-232">Exemplo</span><span class="sxs-lookup"><span data-stu-id="038ff-232">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="038ff-233">Nenhum prefixo</span><span class="sxs-lookup"><span data-stu-id="038ff-233">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="038ff-234">Traço único (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="038ff-234">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="038ff-235">Dois traços (`--`)</span><span class="sxs-lookup"><span data-stu-id="038ff-235">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="038ff-236">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="038ff-236">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="038ff-237">&#8224;Uma chave com um prefixo de traço único (`-`) deve ser fornecida em [mapeamentos de comutador](#switch-mappings), como descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="038ff-237">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="038ff-238">Comando de exemplo:</span><span class="sxs-lookup"><span data-stu-id="038ff-238">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="038ff-239">Observação: se `-key1` não estiver presente nos [mapeamentos de comutador](#switch-mappings) fornecidos para o provedor de configuração, uma `FormatException` será gerada.</span><span class="sxs-lookup"><span data-stu-id="038ff-239">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="038ff-240">**Sequência de dois argumentos**</span><span class="sxs-lookup"><span data-stu-id="038ff-240">**Sequence of two arguments**</span></span>

<span data-ttu-id="038ff-241">O valor não pode ser nulo e deve seguir a chave separada por um espaço.</span><span class="sxs-lookup"><span data-stu-id="038ff-241">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="038ff-242">A chave deve ter um prefixo.</span><span class="sxs-lookup"><span data-stu-id="038ff-242">The key must have a prefix.</span></span>

| <span data-ttu-id="038ff-243">Prefixo da chave</span><span class="sxs-lookup"><span data-stu-id="038ff-243">Key prefix</span></span>               | <span data-ttu-id="038ff-244">Exemplo</span><span class="sxs-lookup"><span data-stu-id="038ff-244">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="038ff-245">Traço único (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="038ff-245">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="038ff-246">Dois traços (`--`)</span><span class="sxs-lookup"><span data-stu-id="038ff-246">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="038ff-247">Barra (`/`)</span><span class="sxs-lookup"><span data-stu-id="038ff-247">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="038ff-248">&#8224;Uma chave com um prefixo de traço único (`-`) deve ser fornecida em [mapeamentos de comutador](#switch-mappings), como descrito abaixo.</span><span class="sxs-lookup"><span data-stu-id="038ff-248">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="038ff-249">Comando de exemplo:</span><span class="sxs-lookup"><span data-stu-id="038ff-249">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="038ff-250">Observação: se `-key1` não estiver presente nos [mapeamentos de comutador](#switch-mappings) fornecidos para o provedor de configuração, uma `FormatException` será gerada.</span><span class="sxs-lookup"><span data-stu-id="038ff-250">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="038ff-251">Chaves duplicadas</span><span class="sxs-lookup"><span data-stu-id="038ff-251">Duplicate keys</span></span>

<span data-ttu-id="038ff-252">Se chaves duplicadas forem fornecidas, o último par chave-valor será usado.</span><span class="sxs-lookup"><span data-stu-id="038ff-252">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="038ff-253">Mapeamentos de comutador</span><span class="sxs-lookup"><span data-stu-id="038ff-253">Switch mappings</span></span>

<span data-ttu-id="038ff-254">Ao criar manualmente a configuração com `ConfigurationBuilder`, um dicionário de mapeamentos de comutador pode ser adicionado ao método `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="038ff-254">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="038ff-255">Os mapeamentos de comutador permitem fornecer a lógica de substituição do nome da chave.</span><span class="sxs-lookup"><span data-stu-id="038ff-255">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="038ff-256">Ao ser usado, o dicionário de mapeamentos de comutador é verificado para oferecer uma chave que corresponda à chave fornecida por um argumento de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="038ff-256">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="038ff-257">Se a chave de linha de comando for encontrada no dicionário, o valor do dicionário (a substituição da chave) será passado de volta para definir a configuração.</span><span class="sxs-lookup"><span data-stu-id="038ff-257">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="038ff-258">Um mapeamento de comutador é necessário para qualquer chave de linha de comando prefixada com um traço único (`-`).</span><span class="sxs-lookup"><span data-stu-id="038ff-258">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="038ff-259">Regras de chave do dicionário de mapeamentos de comutador:</span><span class="sxs-lookup"><span data-stu-id="038ff-259">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="038ff-260">Os comutadores devem começar com um traço (`-`) ou traço duplo (`--`).</span><span class="sxs-lookup"><span data-stu-id="038ff-260">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="038ff-261">O dicionário de mapeamentos de comutador chave não deve conter chaves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="038ff-261">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="038ff-262">No exemplo a seguir, o método `GetSwitchMappings` permite que os argumentos de linha de comando usem um prefixo de chave de traço único (`-`) e evitem prefixos de subchaves à esquerda.</span><span class="sxs-lookup"><span data-stu-id="038ff-262">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="038ff-263">Sem fornecer argumentos de linha de comando, o dicionário fornecido para `AddInMemoryCollection` definirá os valores de configuração.</span><span class="sxs-lookup"><span data-stu-id="038ff-263">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="038ff-264">Execute o aplicativo com o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="038ff-264">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="038ff-265">A janela do console exibe:</span><span class="sxs-lookup"><span data-stu-id="038ff-265">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="038ff-266">Use o seguinte para passar parâmetros de configuração:</span><span class="sxs-lookup"><span data-stu-id="038ff-266">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="038ff-267">A janela do console exibe:</span><span class="sxs-lookup"><span data-stu-id="038ff-267">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="038ff-268">Depois que o dicionário de mapeamentos de comutador for criado, ele conterá os dados mostrados na tabela a seguir:</span><span class="sxs-lookup"><span data-stu-id="038ff-268">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="038ff-269">Chave</span><span class="sxs-lookup"><span data-stu-id="038ff-269">Key</span></span>            | <span data-ttu-id="038ff-270">Valor</span><span class="sxs-lookup"><span data-stu-id="038ff-270">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="038ff-271">Para demonstrar a troca de chaves usando o dicionário, execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="038ff-271">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="038ff-272">As chaves da linha de comando são trocadas.</span><span class="sxs-lookup"><span data-stu-id="038ff-272">The command-line keys are swapped.</span></span> <span data-ttu-id="038ff-273">A janela de console exibe os valores de configuração de `Profile:MachineName` e `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="038ff-273">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="038ff-274">Arquivo web.config</span><span class="sxs-lookup"><span data-stu-id="038ff-274">web.config file</span></span>

<span data-ttu-id="038ff-275">Um arquivo *web.config* é necessário quando você hospeda o aplicativo em IIS ou IIS Express.</span><span class="sxs-lookup"><span data-stu-id="038ff-275">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="038ff-276">As configurações no *web.config* habilitam o [Módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para iniciar o aplicativo e definir outras configurações e módulos do IIS.</span><span class="sxs-lookup"><span data-stu-id="038ff-276">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="038ff-277">Se o arquivo *web.config* não estiver presente e o arquivo de projeto inclui `<Project Sdk="Microsoft.NET.Sdk.Web">`, a publicação do projeto criará um arquivo *web.config* na saída publicada (a pasta *publish*).</span><span class="sxs-lookup"><span data-stu-id="038ff-277">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="038ff-278">Para obter mais informações, consulte [Hospedar o ASP.NET Core no Windows com o IIS](xref:host-and-deploy/iis/index#webconfig-file).</span><span class="sxs-lookup"><span data-stu-id="038ff-278">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="accessing-configuration-during-startup"></a><span data-ttu-id="038ff-279">Acessando a configuração durante a inicialização</span><span class="sxs-lookup"><span data-stu-id="038ff-279">Accessing configuration during startup</span></span>

<span data-ttu-id="038ff-280">Para acessar a configuração em `ConfigureServices` ou `Configure` durante a inicialização, consulte os exemplos no tópico [Inicialização do aplicativo](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="038ff-280">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="038ff-281">Observações adicionais</span><span class="sxs-lookup"><span data-stu-id="038ff-281">Additional notes</span></span>

* <span data-ttu-id="038ff-282">A DI (injeção de dependência) não é configurada até que `ConfigureServices` seja invocado.</span><span class="sxs-lookup"><span data-stu-id="038ff-282">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="038ff-283">O sistema de configuração não tem reconhecimento de DI.</span><span class="sxs-lookup"><span data-stu-id="038ff-283">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="038ff-284">O `IConfiguration` tem duas especializações:</span><span class="sxs-lookup"><span data-stu-id="038ff-284">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="038ff-285">`IConfigurationRoot` Usado para o nó raiz.</span><span class="sxs-lookup"><span data-stu-id="038ff-285">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="038ff-286">Pode disparar um recarregamento.</span><span class="sxs-lookup"><span data-stu-id="038ff-286">Can trigger a reload.</span></span>
  * <span data-ttu-id="038ff-287">`IConfigurationSection` Representa uma seção de valores de configuração.</span><span class="sxs-lookup"><span data-stu-id="038ff-287">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="038ff-288">O métodos `GetSection` e `GetChildren` retornam um `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="038ff-288">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="038ff-289">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) ao recarregar a configuração ou para ter acesso a cada provedor.</span><span class="sxs-lookup"><span data-stu-id="038ff-289">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="038ff-290">Nenhuma dessas situações são comuns.</span><span class="sxs-lookup"><span data-stu-id="038ff-290">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="038ff-291">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="038ff-291">Additional resources</span></span>

* [<span data-ttu-id="038ff-292">Opções</span><span class="sxs-lookup"><span data-stu-id="038ff-292">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="038ff-293">Trabalhando com vários ambientes</span><span class="sxs-lookup"><span data-stu-id="038ff-293">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="038ff-294">Armazenamento seguro dos segredos do aplicativo durante o desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="038ff-294">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="038ff-295">Hospedagem no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="038ff-295">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="038ff-296">Injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="038ff-296">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="038ff-297">Provedor de configuração do Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="038ff-297">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)

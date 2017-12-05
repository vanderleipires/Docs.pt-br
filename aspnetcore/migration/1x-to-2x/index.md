---
title: Migrando do ASP.NET Core 1.x para 2.0
author: scottaddie
description: "Este artigo descreve os pré-requisitos e as etapas mais comuns para a migração de um projeto ASP.NET Core 1.x para o ASP.NET Core 2.0."
keywords: ASP.NET Core, migrando
ms.author: scaddie
manager: wpickett
ms.date: 10/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/index
ms.openlocfilehash: 12734504953f2942458c3bfe1fe146f48d8f24ff
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2017
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a><span data-ttu-id="1a002-104">Migrando do ASP.NET Core 1.x para o ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="1a002-104">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>

<span data-ttu-id="1a002-105">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="1a002-105">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="1a002-106">Neste artigo, vamos orientá-lo pela atualização de um projeto existente ASP.NET Core 1.x para o ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="1a002-106">In this article, we'll walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="1a002-107">A migração do aplicativo para o ASP.NET Core 2.0 permite que você aproveite [muitos novos recursos e melhorias de desempenho](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="1a002-107">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span></span> 

<span data-ttu-id="1a002-108">Os aplicativos ASP.NET Core 1.x existentes baseiam-se em modelos de projeto específicos à versão.</span><span class="sxs-lookup"><span data-stu-id="1a002-108">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="1a002-109">Conforme a estrutura do ASP.NET Core evolui, os modelos do projeto e o código inicial contido neles também.</span><span class="sxs-lookup"><span data-stu-id="1a002-109">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="1a002-110">Além de atualizar a estrutura ASP.NET Core, você precisa atualizar o código para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1a002-110">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="1a002-111">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="1a002-111">Prerequisites</span></span>
<span data-ttu-id="1a002-112">Consulte [Introdução ao ASP.NET Core](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="1a002-112">Please see [Getting Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="1a002-113">Atualizar TFM (Moniker da Estrutura de Destino)</span><span class="sxs-lookup"><span data-stu-id="1a002-113">Update Target Framework Moniker (TFM)</span></span>
<span data-ttu-id="1a002-114">Projetos direcionados ao .NET Core devem usar o [TFM](/dotnet/standard/frameworks#referring-to-frameworks) de uma versão maior ou igual ao .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="1a002-114">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="1a002-115">Pesquise o nó `<TargetFramework>` no arquivo *.csproj* e substitua seu texto interno por `netcoreapp2.0`:</span><span class="sxs-lookup"><span data-stu-id="1a002-115">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

<span data-ttu-id="1a002-116">Projetos direcionados ao .NET Framework devem usar o TFM de uma versão maior ou igual ao .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="1a002-116">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="1a002-117">Pesquise o nó `<TargetFramework>` no arquivo *.csproj* e substitua seu texto interno por `net461`:</span><span class="sxs-lookup"><span data-stu-id="1a002-117">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> <span data-ttu-id="1a002-118">O .NET Core 2.0 oferece uma área de superfície muito maior do que o .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="1a002-118">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="1a002-119">Se você estiver direcionando o .NET Framework exclusivamente devido a APIs ausentes no .NET Core 1.x, o direcionamento do .NET Core 2.0 provavelmente funcionará.</span><span class="sxs-lookup"><span data-stu-id="1a002-119">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="1a002-120">Atualizar a versão do SDK do .NET Core em global.json</span><span class="sxs-lookup"><span data-stu-id="1a002-120">Update .NET Core SDK version in global.json</span></span>
<span data-ttu-id="1a002-121">Se a solução depender de um arquivo [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) para direcionar uma versão específica do SDK do .NET Core, atualize sua propriedade `version` para que ela use a versão 2.0 instalada no computador:</span><span class="sxs-lookup"><span data-stu-id="1a002-121">If your solution relies upon a [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="1a002-122">Referências do pacote de atualização</span><span class="sxs-lookup"><span data-stu-id="1a002-122">Update package references</span></span>
<span data-ttu-id="1a002-123">O arquivo *.csproj* em um projeto 1.x lista cada pacote NuGet usado pelo projeto.</span><span class="sxs-lookup"><span data-stu-id="1a002-123">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="1a002-124">Em um projeto ASP.NET Core 2.0 direcionado ao .NET Core 2.0, uma única referência de [metapacote](xref:fundamentals/metapackage) no arquivo *.csproj* substitui a coleção de pacotes:</span><span class="sxs-lookup"><span data-stu-id="1a002-124">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

<span data-ttu-id="1a002-125">Todos os recursos do ASP.NET Core 2.0 e do Entity Framework Core 2.0 são incluídos no metapacote.</span><span class="sxs-lookup"><span data-stu-id="1a002-125">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="1a002-126">Os projetos do ASP.NET Core 2.0 direcionados ao .NET Framework devem continuar a referenciar pacotes NuGet individuais.</span><span class="sxs-lookup"><span data-stu-id="1a002-126">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="1a002-127">Atualize o atributo `Version` de cada nó `<PackageReference />` para 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="1a002-127">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="1a002-128">Por exemplo, esta é a lista de nós `<PackageReference />` usados em um projeto ASP.NET Core 2.0 típico direcionado ao .NET Framework:</span><span class="sxs-lookup"><span data-stu-id="1a002-128">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="1a002-129">Atualizar ferramentas da CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="1a002-129">Update .NET Core CLI tools</span></span>
<span data-ttu-id="1a002-130">No arquivo *.csproj*, atualize o atributo `Version` de cada nó `<DotNetCliToolReference />` para 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="1a002-130">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="1a002-131">Por exemplo, esta é a lista de ferramentas da CLI usadas em um projeto ASP.NET Core 2.0 típico direcionado ao .NET Core 2.0:</span><span class="sxs-lookup"><span data-stu-id="1a002-131">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="1a002-132">Renomear a propriedade de Fallback de Destino do Pacote</span><span class="sxs-lookup"><span data-stu-id="1a002-132">Rename Package Target Fallback property</span></span>
<span data-ttu-id="1a002-133">O arquivo *.csproj* de um projeto 1.x usou um nó `PackageTargetFallback` e uma variável:</span><span class="sxs-lookup"><span data-stu-id="1a002-133">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

<span data-ttu-id="1a002-134">Renomeie o nó e a variável como `AssetTargetFallback`:</span><span class="sxs-lookup"><span data-stu-id="1a002-134">Rename both the node and variable to `AssetTargetFallback`:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="1a002-135">Atualizar o método Main em Program.cs</span><span class="sxs-lookup"><span data-stu-id="1a002-135">Update Main method in Program.cs</span></span>
<span data-ttu-id="1a002-136">Em projetos 1.x, o método `Main` de *Program.cs* era parecido com este:</span><span class="sxs-lookup"><span data-stu-id="1a002-136">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

<span data-ttu-id="1a002-137">Em projetos 2.0, o método `Main` de *Program.cs* foi simplificado:</span><span class="sxs-lookup"><span data-stu-id="1a002-137">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

<span data-ttu-id="1a002-138">A adoção desse novo padrão do 2.0 é altamente recomendada e é necessária para que os recursos de produto como as [Migrações do Entity Framework (EF) Core](xref:data/ef-mvc/migrations) funcionem.</span><span class="sxs-lookup"><span data-stu-id="1a002-138">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="1a002-139">Por exemplo, a execução de `Update-Database` na janela do Console do Gerenciador de Pacotes ou de `dotnet ef database update` na linha de comando (em projetos convertidos no ASP.NET Core 2.0) gera o seguinte erro:</span><span class="sxs-lookup"><span data-stu-id="1a002-139">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a><span data-ttu-id="1a002-140">Adicionar provedores de configuração</span><span class="sxs-lookup"><span data-stu-id="1a002-140">Add configuration providers</span></span>
<span data-ttu-id="1a002-141">Nos projetos 1.x, a adição de provedores de configuração em um aplicativo foi realizada por meio do construtor `Startup`.</span><span class="sxs-lookup"><span data-stu-id="1a002-141">In 1.x projects, adding configuration providers to an app was accomplished via the `Startup` constructor.</span></span> <span data-ttu-id="1a002-142">As etapas incluíram a criação de uma instância de `ConfigurationBuilder`, o carregamento de provedores aplicáveis (variáveis de ambiente, configurações do aplicativo, etc.) e a inicialização de um membro de `IConfigurationRoot`.</span><span class="sxs-lookup"><span data-stu-id="1a002-142">The steps involved creating an instance of `ConfigurationBuilder`, loading applicable providers (environment variables, app settings, etc.), and initializing a member of `IConfigurationRoot`.</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

<span data-ttu-id="1a002-143">O exemplo anterior carrega o membro `Configuration` com definições de configuração do *appsettings.json*, bem como qualquer arquivo *appsettings.\<EnvironmentName\>.json* correspondente à propriedade `IHostingEnvironment.EnvironmentName`.</span><span class="sxs-lookup"><span data-stu-id="1a002-143">The preceding example loads the `Configuration` member with configuration settings from *appsettings.json* as well as any *appsettings.\<EnvironmentName\>.json* file matching the `IHostingEnvironment.EnvironmentName` property.</span></span> <span data-ttu-id="1a002-144">Estes arquivos estão localizados no mesmo caminho que *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="1a002-144">The location of these files is at the same path as *Startup.cs*.</span></span>

<span data-ttu-id="1a002-145">Nos projetos 2.0, o código de configuração clichê inerente aos projetos 1.x é executado de modo oculto.</span><span class="sxs-lookup"><span data-stu-id="1a002-145">In 2.0 projects, the boilerplate configuration code inherent to 1.x projects runs behind-the-scenes.</span></span> <span data-ttu-id="1a002-146">Por exemplo, as variáveis de ambiente e as configurações do aplicativo são carregadas na inicialização.</span><span class="sxs-lookup"><span data-stu-id="1a002-146">For example, environment variables and app settings are loaded at startup.</span></span> <span data-ttu-id="1a002-147">O código *Startup.cs* equivalente é reduzido à inicialização `IConfiguration` com a instância injetada:</span><span class="sxs-lookup"><span data-stu-id="1a002-147">The equivalent *Startup.cs* code is reduced to `IConfiguration` initialization with the injected instance:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

<span data-ttu-id="1a002-148">Para remover os provedores padrão adicionados por `WebHostBuilder.CreateDefaultBuilder`, invoque o método `Clear` na propriedade `IConfigurationBuilder.Sources` dentro de `ConfigureAppConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="1a002-148">To remove the default providers added by `WebHostBuilder.CreateDefaultBuilder`, invoke the `Clear` method on the `IConfigurationBuilder.Sources` property inside of `ConfigureAppConfiguration`.</span></span> <span data-ttu-id="1a002-149">Para adicionar os provedores de volta, utilize o método `ConfigureAppConfiguration` em *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1a002-149">To add providers back, utilize the `ConfigureAppConfiguration` method in *Program.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

<span data-ttu-id="1a002-150">A configuração usada pelo método `CreateDefaultBuilder` no trecho de código anterior pode ser vista [aqui](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="1a002-150">The configuration used by the `CreateDefaultBuilder` method in the preceding code snippet can be seen [here](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span></span>

<span data-ttu-id="1a002-151">Para obter mais informações, consulte [Configuração no ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="1a002-151">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a><span data-ttu-id="1a002-152">Mover código de inicialização do banco de dados</span><span class="sxs-lookup"><span data-stu-id="1a002-152">Move database initialization code</span></span>
<span data-ttu-id="1a002-153">Em projetos do 1.x usando o EF Core 1.x, um comando como `dotnet ef migrations add` faz o seguinte:</span><span class="sxs-lookup"><span data-stu-id="1a002-153">In 1.x projects using EF Core 1.x, a command such as `dotnet ef migrations add` does the following:</span></span>
1. <span data-ttu-id="1a002-154">Cria uma instância `Startup`</span><span class="sxs-lookup"><span data-stu-id="1a002-154">Instantiates a `Startup` instance</span></span>
2. <span data-ttu-id="1a002-155">Invoca o método `ConfigureServices` para registrar todos os serviços com injeção de dependência (incluindo tipos `DbContext`)</span><span class="sxs-lookup"><span data-stu-id="1a002-155">Invokes the `ConfigureServices` method to register all services with dependency injection (including `DbContext` types)</span></span>
3. <span data-ttu-id="1a002-156">Executa suas tarefas de requisito</span><span class="sxs-lookup"><span data-stu-id="1a002-156">Performs its requisite tasks</span></span>

<span data-ttu-id="1a002-157">Em projetos do 2.0 usando o EF Core 2.0, o `Program.BuildWebHost` é chamado para obter os serviços do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="1a002-157">In 2.0 projects using EF Core 2.0, `Program.BuildWebHost` is invoked to obtain the application services.</span></span> <span data-ttu-id="1a002-158">Ao contrário do 1.x, isso tem o efeito colateral adicional de chamar o `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="1a002-158">Unlike 1.x, this has the additional side effect of invoking `Startup.Configure`.</span></span> <span data-ttu-id="1a002-159">Caso seu aplicativo do 1.x tenha chamado o código de inicialização do banco de dados no seu método `Configure`, problemas inesperados poderão ocorrer.</span><span class="sxs-lookup"><span data-stu-id="1a002-159">If your 1.x app invoked database initialization code in its `Configure` method, unexpected problems can occur.</span></span> <span data-ttu-id="1a002-160">Por exemplo, se o banco de dados não existir ainda, o código de propagação será executado antes da execução do comando EF Core Migrations.</span><span class="sxs-lookup"><span data-stu-id="1a002-160">For example, if the database doesn't yet exist, the seeding code runs before the EF Core Migrations command execution.</span></span> <span data-ttu-id="1a002-161">Esse problema fará com que um comando `dotnet ef migrations list` falhe se o banco de dados ainda não existir.</span><span class="sxs-lookup"><span data-stu-id="1a002-161">This problem causes a `dotnet ef migrations list` command to fail if the database doesn't yet exist.</span></span>

<span data-ttu-id="1a002-162">Considere o seguinte código de inicialização de propagação do 1.x no método `Configure` de *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1a002-162">Consider the following 1.x seed initialization code in the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

<span data-ttu-id="1a002-163">Em projetos do 2.0, mova a chamada `SeedData.Initialize` para o método `Main` do *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1a002-163">In 2.0 projects, move the `SeedData.Initialize` call to the `Main` method of *Program.cs*:</span></span>

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

<span data-ttu-id="1a002-164">A partir do 2.0, é uma prática inadequada realizar qualquer ação no `BuildWebHost`, exceto compilação e configuração do host da Web.</span><span class="sxs-lookup"><span data-stu-id="1a002-164">As of 2.0, it's bad practice to do anything in `BuildWebHost` except build and configure the web host.</span></span> <span data-ttu-id="1a002-165">Tudo relacionado à execução do aplicativo deve ser tratado fora do `BuildWebHost` &mdash;, normalmente no método `Main` do *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="1a002-165">Anything that is about running the application should be handled outside of `BuildWebHost` &mdash; typically in the `Main` method of *Program.cs*.</span></span>

<a name="view-compilation"></a>

## <a name="review-your-razor-view-compilation-setting"></a><span data-ttu-id="1a002-166">Examinar a configuração Compilação de Exibição do Razor</span><span class="sxs-lookup"><span data-stu-id="1a002-166">Review your Razor View Compilation setting</span></span>
<span data-ttu-id="1a002-167">Um tempo de inicialização mais rápido do aplicativo e pacotes publicados menores são de extrema importância para você.</span><span class="sxs-lookup"><span data-stu-id="1a002-167">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="1a002-168">Por esses motivos, a opção [Compilação de exibição do Razor](xref:mvc/views/view-compilation) é habilitada por padrão no ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="1a002-168">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="1a002-169">A configuração da propriedade `MvcRazorCompileOnPublish` como verdadeiro não é mais necessária.</span><span class="sxs-lookup"><span data-stu-id="1a002-169">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="1a002-170">A menos que você esteja desabilitando a compilação de exibição, a propriedade poderá ser removida do arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="1a002-170">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="1a002-171">Ao direcionar o .NET Framework, você ainda precisa referenciar explicitamente o pacote NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) no arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="1a002-171">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="1a002-172">Depender dos recursos “Light-Up” do Application Insights</span><span class="sxs-lookup"><span data-stu-id="1a002-172">Rely on Application Insights "Light-Up" features</span></span>
<span data-ttu-id="1a002-173">A instalação fácil da instrumentação de desempenho do aplicativo é importante.</span><span class="sxs-lookup"><span data-stu-id="1a002-173">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="1a002-174">Agora, você pode contar com os novos recursos “light-up” do [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) disponíveis nas ferramentas do Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="1a002-174">You can now rely on the new [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="1a002-175">Os projetos do ASP.NET Core 1.1 criados no Visual Studio 2017 adicionaram o Application Insights por padrão.</span><span class="sxs-lookup"><span data-stu-id="1a002-175">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="1a002-176">Se não estiver usando o SDK do Application Insights diretamente, fora de *Program.cs* e *Startup.cs*, siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="1a002-176">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="1a002-177">Se você estiver direcionando ao .NET Core, remova o nó `<PackageReference />` seguinte do arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="1a002-177">If targeting .NET Core, remove the following `<PackageReference />` node from the *.csproj* file:</span></span>
    
    [!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. <span data-ttu-id="1a002-178">Se você estiver direcionando ao .NET Core, remova a invocação do método de extensão `UseApplicationInsights` de *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1a002-178">If targeting .NET Core, remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    [!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. <span data-ttu-id="1a002-179">Remova a chamada à API do lado do cliente do Application Insights de *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1a002-179">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="1a002-180">Ela inclui as duas seguintes linhas de código:</span><span class="sxs-lookup"><span data-stu-id="1a002-180">It comprises the following two lines of code:</span></span>

    [!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

<span data-ttu-id="1a002-181">Se você estiver usando o SDK do Application Insights diretamente, continue fazendo isso.</span><span class="sxs-lookup"><span data-stu-id="1a002-181">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="1a002-182">O [metapacote](xref:fundamentals/metapackage) do 2.0 inclui a última versão do Application Insights. Portanto, um erro de downgrade do pacote será exibido se você estiver referenciando uma versão mais antiga.</span><span class="sxs-lookup"><span data-stu-id="1a002-182">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authentication--identity-improvements"></a><span data-ttu-id="1a002-183">Adotar melhorias de autenticação/identidade</span><span class="sxs-lookup"><span data-stu-id="1a002-183">Adopt Authentication / Identity Improvements</span></span>
<span data-ttu-id="1a002-184">O ASP.NET Core 2.0 tem um novo modelo de autenticação e uma série de alterações significativas no ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="1a002-184">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="1a002-185">Se você criou o projeto com a opção Contas de Usuário Individuais habilitada ou se adicionou manualmente a autenticação ou a identidade, consulte [Migrando a autenticação e a identidade para o ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span><span class="sxs-lookup"><span data-stu-id="1a002-185">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1a002-186">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="1a002-186">Additional Resources</span></span>
- [<span data-ttu-id="1a002-187">Últimas alterações no ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="1a002-187">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)

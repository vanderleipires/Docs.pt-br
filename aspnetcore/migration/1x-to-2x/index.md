---
title: Migrando do ASP.NET Core 1.x para 2.0
author: scottaddie
description: "Este artigo descreve os pré-requisitos e as etapas mais comuns para a migração de um projeto ASP.NET Core 1.x para o ASP.NET Core 2.0."
keywords: ASP.NET Core, migrando
ms.author: scaddie
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/index
ms.openlocfilehash: db277ee6b079eab973a565983d6661bf95fce2e3
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a><span data-ttu-id="ed392-104">Migrando do ASP.NET Core 1.x para o ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="ed392-104">Migrating from ASP.NET Core 1.x to ASP.NET Core 2.0</span></span>

<span data-ttu-id="ed392-105">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="ed392-105">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="ed392-106">Neste artigo, vamos orientá-lo pela atualização de um projeto existente ASP.NET Core 1.x para o ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="ed392-106">In this article, we'll walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="ed392-107">A migração do aplicativo para o ASP.NET Core 2.0 permite que você aproveite [muitos novos recursos e melhorias de desempenho](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="ed392-107">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span></span> 

<span data-ttu-id="ed392-108">Os aplicativos ASP.NET Core 1.x existentes baseiam-se em modelos de projeto específicos à versão.</span><span class="sxs-lookup"><span data-stu-id="ed392-108">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="ed392-109">Conforme a estrutura do ASP.NET Core evolui, os modelos do projeto e o código inicial contido neles também.</span><span class="sxs-lookup"><span data-stu-id="ed392-109">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="ed392-110">Além de atualizar a estrutura ASP.NET Core, você precisa atualizar o código para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ed392-110">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="ed392-111">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="ed392-111">Prerequisites</span></span>
<span data-ttu-id="ed392-112">Consulte [Introdução ao ASP.NET Core](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="ed392-112">Please see [Getting Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="ed392-113">Atualizar TFM (Moniker da Estrutura de Destino)</span><span class="sxs-lookup"><span data-stu-id="ed392-113">Update Target Framework Moniker (TFM)</span></span>
<span data-ttu-id="ed392-114">Projetos direcionados ao .NET Core devem usar o [TFM](/dotnet/standard/frameworks#referring-to-frameworks) de uma versão maior ou igual ao .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="ed392-114">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="ed392-115">Pesquise o nó `<TargetFramework>` no arquivo *.csproj* e substitua seu texto interno por `netcoreapp2.0`:</span><span class="sxs-lookup"><span data-stu-id="ed392-115">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

<span data-ttu-id="ed392-116">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]</span><span class="sxs-lookup"><span data-stu-id="ed392-116">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]</span></span>

<span data-ttu-id="ed392-117">Projetos direcionados ao .NET Framework devem usar o TFM de uma versão maior ou igual ao .NET Framework 4.6.1.</span><span class="sxs-lookup"><span data-stu-id="ed392-117">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="ed392-118">Pesquise o nó `<TargetFramework>` no arquivo *.csproj* e substitua seu texto interno por `net461`:</span><span class="sxs-lookup"><span data-stu-id="ed392-118">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

<span data-ttu-id="ed392-119">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]</span><span class="sxs-lookup"><span data-stu-id="ed392-119">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]</span></span>

> [!NOTE]
> <span data-ttu-id="ed392-120">O .NET Core 2.0 oferece uma área de superfície muito maior do que o .NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="ed392-120">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="ed392-121">Se você estiver direcionando o .NET Framework exclusivamente devido a APIs ausentes no .NET Core 1.x, o direcionamento do .NET Core 2.0 provavelmente funcionará.</span><span class="sxs-lookup"><span data-stu-id="ed392-121">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="ed392-122">Atualizar a versão do SDK do .NET Core em global.json</span><span class="sxs-lookup"><span data-stu-id="ed392-122">Update .NET Core SDK version in global.json</span></span>
<span data-ttu-id="ed392-123">Se a solução depender de um arquivo [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) para direcionar uma versão específica do SDK do .NET Core, atualize sua propriedade `version` para que ela use a versão 2.0 instalada no computador:</span><span class="sxs-lookup"><span data-stu-id="ed392-123">If your solution relies upon a [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

<span data-ttu-id="ed392-124">[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="ed392-124">[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]</span></span>

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="ed392-125">Referências do pacote de atualização</span><span class="sxs-lookup"><span data-stu-id="ed392-125">Update package references</span></span>
<span data-ttu-id="ed392-126">O arquivo *.csproj* em um projeto 1.x lista cada pacote NuGet usado pelo projeto.</span><span class="sxs-lookup"><span data-stu-id="ed392-126">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="ed392-127">Em um projeto ASP.NET Core 2.0 direcionado ao .NET Core 2.0, uma única referência de [metapacote](xref:fundamentals/metapackage) no arquivo *.csproj* substitui a coleção de pacotes:</span><span class="sxs-lookup"><span data-stu-id="ed392-127">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

<span data-ttu-id="ed392-128">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]</span><span class="sxs-lookup"><span data-stu-id="ed392-128">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]</span></span>

<span data-ttu-id="ed392-129">Todos os recursos do ASP.NET Core 2.0 e do Entity Framework Core 2.0 são incluídos no metapacote.</span><span class="sxs-lookup"><span data-stu-id="ed392-129">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="ed392-130">Os projetos do ASP.NET Core 2.0 direcionados ao .NET Framework devem continuar a referenciar pacotes NuGet individuais.</span><span class="sxs-lookup"><span data-stu-id="ed392-130">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="ed392-131">Atualize o atributo `Version` de cada nó `<PackageReference />` para 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="ed392-131">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="ed392-132">Por exemplo, esta é a lista de nós `<PackageReference />` usados em um projeto ASP.NET Core 2.0 típico direcionado ao .NET Framework:</span><span class="sxs-lookup"><span data-stu-id="ed392-132">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

<span data-ttu-id="ed392-133">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]</span><span class="sxs-lookup"><span data-stu-id="ed392-133">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]</span></span>

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="ed392-134">Atualizar ferramentas da CLI do .NET Core</span><span class="sxs-lookup"><span data-stu-id="ed392-134">Update .NET Core CLI tools</span></span>
<span data-ttu-id="ed392-135">No arquivo *.csproj*, atualize o atributo `Version` de cada nó `<DotNetCliToolReference />` para 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="ed392-135">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="ed392-136">Por exemplo, esta é a lista de ferramentas da CLI usadas em um projeto ASP.NET Core 2.0 típico direcionado ao .NET Core 2.0:</span><span class="sxs-lookup"><span data-stu-id="ed392-136">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

<span data-ttu-id="ed392-137">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]</span><span class="sxs-lookup"><span data-stu-id="ed392-137">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]</span></span>

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="ed392-138">Renomear a propriedade de Fallback de Destino do Pacote</span><span class="sxs-lookup"><span data-stu-id="ed392-138">Rename Package Target Fallback property</span></span>
<span data-ttu-id="ed392-139">O arquivo *.csproj* de um projeto 1.x usou um nó `PackageTargetFallback` e uma variável:</span><span class="sxs-lookup"><span data-stu-id="ed392-139">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

<span data-ttu-id="ed392-140">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]</span><span class="sxs-lookup"><span data-stu-id="ed392-140">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]</span></span>

<span data-ttu-id="ed392-141">Renomeie o nó e a variável como `AssetTargetFallback`:</span><span class="sxs-lookup"><span data-stu-id="ed392-141">Rename both the node and variable to `AssetTargetFallback`:</span></span>

<span data-ttu-id="ed392-142">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]</span><span class="sxs-lookup"><span data-stu-id="ed392-142">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]</span></span>

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="ed392-143">Atualizar o método Main em Program.cs</span><span class="sxs-lookup"><span data-stu-id="ed392-143">Update Main method in Program.cs</span></span>
<span data-ttu-id="ed392-144">Em projetos 1.x, o método `Main` de *Program.cs* era parecido com este:</span><span class="sxs-lookup"><span data-stu-id="ed392-144">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

<span data-ttu-id="ed392-145">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]</span><span class="sxs-lookup"><span data-stu-id="ed392-145">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]</span></span>

<span data-ttu-id="ed392-146">Em projetos 2.0, o método `Main` de *Program.cs* foi simplificado:</span><span class="sxs-lookup"><span data-stu-id="ed392-146">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

<span data-ttu-id="ed392-147">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]</span><span class="sxs-lookup"><span data-stu-id="ed392-147">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]</span></span>

<span data-ttu-id="ed392-148">A adoção deste novo padrão do 2.0 é altamente recomendado e é necessário para que os recursos de produto como as [Migrações do Entity Framework Core](xref:data/ef-mvc/migrations) funcionem.</span><span class="sxs-lookup"><span data-stu-id="ed392-148">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="ed392-149">Por exemplo, a execução de `Update-Database` na janela do Console do Gerenciador de Pacotes ou de `dotnet ef database update` na linha de comando (em projetos convertidos no ASP.NET Core 2.0) gera o seguinte erro:</span><span class="sxs-lookup"><span data-stu-id="ed392-149">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="view-compilation"></a>

## <a name="review-your-razor-view-compilation-setting"></a><span data-ttu-id="ed392-150">Examinar a configuração Compilação de Exibição do Razor</span><span class="sxs-lookup"><span data-stu-id="ed392-150">Review your Razor View Compilation setting</span></span>
<span data-ttu-id="ed392-151">Um tempo de inicialização mais rápido do aplicativo e pacotes publicados menores são de extrema importância para você.</span><span class="sxs-lookup"><span data-stu-id="ed392-151">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="ed392-152">Por esses motivos, a opção [Compilação de exibição do Razor](xref:mvc/views/view-compilation) é habilitada por padrão no ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="ed392-152">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="ed392-153">A configuração da propriedade `MvcRazorCompileOnPublish` como verdadeiro não é mais necessária.</span><span class="sxs-lookup"><span data-stu-id="ed392-153">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="ed392-154">A menos que você esteja desabilitando a compilação de exibição, a propriedade poderá ser removida do arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="ed392-154">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="ed392-155">Ao direcionar o .NET Framework, você ainda precisa referenciar explicitamente o pacote NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) no arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="ed392-155">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

<span data-ttu-id="ed392-156">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]</span><span class="sxs-lookup"><span data-stu-id="ed392-156">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]</span></span>

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="ed392-157">Depender dos recursos “Light-Up” do Application Insights</span><span class="sxs-lookup"><span data-stu-id="ed392-157">Rely on Application Insights "Light-Up" features</span></span>
<span data-ttu-id="ed392-158">A instalação fácil da instrumentação de desempenho do aplicativo é importante.</span><span class="sxs-lookup"><span data-stu-id="ed392-158">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="ed392-159">Agora, você pode contar com os novos recursos “light-up” do [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) disponíveis nas ferramentas do Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="ed392-159">You can now rely on the new [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="ed392-160">Os projetos do ASP.NET Core 1.1 criados no Visual Studio 2017 adicionaram o Application Insights por padrão.</span><span class="sxs-lookup"><span data-stu-id="ed392-160">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="ed392-161">Se não estiver usando o SDK do Application Insights diretamente, fora de *Program.cs* e *Startup.cs*, siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="ed392-161">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="ed392-162">Remova o seguinte nó `<PackageReference />` do arquivo *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="ed392-162">Remove the following `<PackageReference />` node from the *.csproj* file:</span></span>
    
    <span data-ttu-id="ed392-163">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]</span><span class="sxs-lookup"><span data-stu-id="ed392-163">[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]</span></span>

2. <span data-ttu-id="ed392-164">Remova a invocação do método de extensão `UseApplicationInsights` de *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ed392-164">Remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    <span data-ttu-id="ed392-165">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]</span><span class="sxs-lookup"><span data-stu-id="ed392-165">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]</span></span>

3. <span data-ttu-id="ed392-166">Remova a chamada à API do lado do cliente do Application Insights de *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ed392-166">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="ed392-167">Ela inclui as duas seguintes linhas de código:</span><span class="sxs-lookup"><span data-stu-id="ed392-167">It comprises the following two lines of code:</span></span>

    <span data-ttu-id="ed392-168">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]</span><span class="sxs-lookup"><span data-stu-id="ed392-168">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]</span></span>

<span data-ttu-id="ed392-169">Se você estiver usando o SDK do Application Insights diretamente, continue fazendo isso.</span><span class="sxs-lookup"><span data-stu-id="ed392-169">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="ed392-170">O [metapacote](xref:fundamentals/metapackage) do 2.0 inclui a última versão do Application Insights. Portanto, um erro de downgrade do pacote será exibido se você estiver referenciando uma versão mais antiga.</span><span class="sxs-lookup"><span data-stu-id="ed392-170">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authentication--identity-improvements"></a><span data-ttu-id="ed392-171">Adotar melhorias de autenticação/identidade</span><span class="sxs-lookup"><span data-stu-id="ed392-171">Adopt Authentication / Identity Improvements</span></span>
<span data-ttu-id="ed392-172">O ASP.NET Core 2.0 tem um novo modelo de autenticação e uma série de alterações significativas no ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="ed392-172">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="ed392-173">Se você criou o projeto com a opção Contas de Usuário Individuais habilitada ou se adicionou manualmente a autenticação ou a identidade, consulte [Migrando a autenticação e a identidade para o ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span><span class="sxs-lookup"><span data-stu-id="ed392-173">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrating Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ed392-174">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="ed392-174">Additional Resources</span></span>
- [<span data-ttu-id="ed392-175">Últimas alterações no ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="ed392-175">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)

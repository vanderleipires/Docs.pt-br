---
title: SDK do Razor do ASP.NET Core
author: Rick-Anderson
description: Saiba como as Páginas Razor no ASP.NET Core tornam a codificação de cenários centrados em página mais fácil e mais produtiva do que com o uso de MVC.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: mvc/razor-pages/sdk
ms.openlocfilehash: cf0e1873c7ce500ce3b8ad2b3367555bdc41a576
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555528"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="a0d86-103">SDK do Razor do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a0d86-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="a0d86-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a0d86-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a0d86-105">O [!INCLUDE [](~/includes/2.1-SDK.md)] inclui o SDK do MSBuild `Microsoft.NET.Sdk.Razor` (SDK do Razor).</span><span class="sxs-lookup"><span data-stu-id="a0d86-105">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="a0d86-106">O SDK do Razor:</span><span class="sxs-lookup"><span data-stu-id="a0d86-106">The Razor SDK:</span></span>

* <span data-ttu-id="a0d86-107">Padroniza a experiência de criação, empacotamento e publicação de projetos que contêm arquivos [Razor](xref:mvc/views/razor) para projetos baseados no ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="a0d86-107">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="a0d86-108">Inclui um conjunto de destinos, propriedades e itens predefinidos que permitem personalizar a compilação de arquivos Razor.</span><span class="sxs-lookup"><span data-stu-id="a0d86-108">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a0d86-109">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="a0d86-109">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a><span data-ttu-id="a0d86-110">Usando o SDK do Razor</span><span class="sxs-lookup"><span data-stu-id="a0d86-110">Using the Razor SDK</span></span>

<span data-ttu-id="a0d86-111">A maioria dos aplicativos Web não precisam referenciar expressamente o SDK do Razor.</span><span class="sxs-lookup"><span data-stu-id="a0d86-111">Most web apps don't need to expressly reference the Razor SDK.</span></span> 

<span data-ttu-id="a0d86-112">Para usar o SDK do Razor para criar bibliotecas de classe contendo exibições Razor ou Páginas Razor:</span><span class="sxs-lookup"><span data-stu-id="a0d86-112">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="a0d86-113">Use `Microsoft.NET.Sdk.Razor` em vez de `Microsoft.NET.Sdk`:</span><span class="sxs-lookup"><span data-stu-id="a0d86-113">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* <span data-ttu-id="a0d86-114">Normalmente, uma referência de pacote para `Microsoft.AspNetCore.Mvc` é necessária para trazer as dependências adicionais necessárias para criar e compilar Páginas Razor e Exibições Razor.</span><span class="sxs-lookup"><span data-stu-id="a0d86-114">Typically a package reference to `Microsoft.AspNetCore.Mvc` is required to bring in additional dependencies required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="a0d86-115">No mínimo, o projeto precisa adicionar referências de pacote para:</span><span class="sxs-lookup"><span data-stu-id="a0d86-115">At minimum, your project needs to add package references to:</span></span>

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 <span data-ttu-id="a0d86-116">Os pacotes anteriores são incluídos em `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="a0d86-116">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="a0d86-117">A marcação a seguir mostra um arquivo *.csproj* que usa o SDK do Razor para criar arquivos Razor para um aplicativo Páginas Razor do ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="a0d86-117">The following markup shows a basic *.csproj* file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a><span data-ttu-id="a0d86-118">Propriedades</span><span class="sxs-lookup"><span data-stu-id="a0d86-118">Properties</span></span>

<span data-ttu-id="a0d86-119">As seguintes propriedades controlam o comportamento do SDK do Razor como parte de um build de projeto:</span><span class="sxs-lookup"><span data-stu-id="a0d86-119">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="a0d86-120">`RazorCompileOnBuild`: Quando `true`, compila e emite o assembly Razor como parte da criação do projeto.</span><span class="sxs-lookup"><span data-stu-id="a0d86-120">`RazorCompileOnBuild` : When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="a0d86-121">Assume o padrão de `true`.</span><span class="sxs-lookup"><span data-stu-id="a0d86-121">Defaults to `true`.</span></span>
* <span data-ttu-id="a0d86-122">`RazorCompileOnPublish`: quando `true`, compila e emite o assembly Razor como parte da publicação do projeto.</span><span class="sxs-lookup"><span data-stu-id="a0d86-122">`RazorCompileOnPublish` : When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="a0d86-123">Assume o padrão de `true`.</span><span class="sxs-lookup"><span data-stu-id="a0d86-123">Defaults to `true`.</span></span>

<span data-ttu-id="a0d86-124">As propriedades e os itens a seguir são usados para configurar entradas e saídas para o SDK do Razor:</span><span class="sxs-lookup"><span data-stu-id="a0d86-124">The following properties and items are used to configure inputs and output to the Razor SDK:</span></span>

| <span data-ttu-id="a0d86-125">Itens</span><span class="sxs-lookup"><span data-stu-id="a0d86-125">Items</span></span>                                         | <span data-ttu-id="a0d86-126">Descrição</span><span class="sxs-lookup"><span data-stu-id="a0d86-126">Description</span></span>                                                                   |
| ------------                                  | -------------                                                                 |
| <span data-ttu-id="a0d86-127">RazorGenerate</span><span class="sxs-lookup"><span data-stu-id="a0d86-127">RazorGenerate</span></span>                                 | <span data-ttu-id="a0d86-128">Elementos de item (arquivos *.cshtml*) que são entradas para os destinos de geração de código.</span><span class="sxs-lookup"><span data-stu-id="a0d86-128">Item elements (*.cshtml* files) that are inputs to code generation targets.</span></span> |
| <span data-ttu-id="a0d86-129">RazorCompile</span><span class="sxs-lookup"><span data-stu-id="a0d86-129">RazorCompile</span></span>                                  | <span data-ttu-id="a0d86-130">Elementos de item (arquivos .cs) que são entradas para os destinos de compilação do Razor.</span><span class="sxs-lookup"><span data-stu-id="a0d86-130">Item elements (.cs files) that are inputs to  Razor compilation targets.</span></span> <span data-ttu-id="a0d86-131">Use este ItemGroup para especificar arquivos adicionais a serem compilados no assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="a0d86-131">Use this ItemGroup to specify additional files to be compiled into the Razor assembly.</span></span> |
| <span data-ttu-id="a0d86-132">RazorTargetAssemblyAttribute</span><span class="sxs-lookup"><span data-stu-id="a0d86-132">RazorTargetAssemblyAttribute</span></span>                  | <span data-ttu-id="a0d86-133">Os elementos de item usados para a codificação geram atributos para o assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="a0d86-133">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="a0d86-134">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="a0d86-134">For example:</span></span>  <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| <span data-ttu-id="a0d86-135">RazorEmbeddedResource</span><span class="sxs-lookup"><span data-stu-id="a0d86-135">RazorEmbeddedResource</span></span>                         | <span data-ttu-id="a0d86-136">Elementos de item adicionados como recursos incorporados ao assembly Razor gerado</span><span class="sxs-lookup"><span data-stu-id="a0d86-136">Item elements added as embedded resources to the generated Razor assembly</span></span> |

| <span data-ttu-id="a0d86-137">propriedade</span><span class="sxs-lookup"><span data-stu-id="a0d86-137">Property</span></span>                                      | <span data-ttu-id="a0d86-138">Descrição</span><span class="sxs-lookup"><span data-stu-id="a0d86-138">Description</span></span>                                                                   |
| ------------                                  | -------------                                                                 |
| <span data-ttu-id="a0d86-139">RazorTargetName</span><span class="sxs-lookup"><span data-stu-id="a0d86-139">RazorTargetName</span></span>                               | <span data-ttu-id="a0d86-140">Nome do arquivo (sem extensão) do assembly produzido pelo Razor.</span><span class="sxs-lookup"><span data-stu-id="a0d86-140">File name (without extension) of the assembly produced by Razor.</span></span> | 
| <span data-ttu-id="a0d86-141">RazorOutputPath</span><span class="sxs-lookup"><span data-stu-id="a0d86-141">RazorOutputPath</span></span>                               | <span data-ttu-id="a0d86-142">O diretório de saída do Razor.</span><span class="sxs-lookup"><span data-stu-id="a0d86-142">The Razor output directory.</span></span>                                      |
| <span data-ttu-id="a0d86-143">RazorCompileToolset</span><span class="sxs-lookup"><span data-stu-id="a0d86-143">RazorCompileToolset</span></span>                           | <span data-ttu-id="a0d86-144">Usado para determinar o conjunto de ferramentas usado para criar o assembly do Razor.</span><span class="sxs-lookup"><span data-stu-id="a0d86-144">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="a0d86-145">Os valores válidos são `Implicit` e `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="a0d86-145">Valid values are `Implicit`, , and `PrecompilationTool`.</span></span> |
| <span data-ttu-id="a0d86-146">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="a0d86-146">EnableDefaultContentItems</span></span>                     | <span data-ttu-id="a0d86-147">Quando `true`, inclui a determinados tipos de arquivo, como arquivos *.cshtml*, como conteúdo do projeto.</span><span class="sxs-lookup"><span data-stu-id="a0d86-147">When `true`, includes certain file types, such as *.cshtml* files, as content in the project.</span></span> <span data-ttu-id="a0d86-148">Quando referenciado por meio de Microsoft.NET.Sdk.Web, também inclui todos os arquivos em *wwwroot* e arquivos de configuração.</span><span class="sxs-lookup"><span data-stu-id="a0d86-148">When referenced via Microsoft.NET.Sdk.Web, also includes all files under *wwwroot*, and config files.</span></span>         |
| <span data-ttu-id="a0d86-149">EnableDefaultRazorGenerateItems</span><span class="sxs-lookup"><span data-stu-id="a0d86-149">EnableDefaultRazorGenerateItems</span></span>               | <span data-ttu-id="a0d86-150">Quando `true`, inclui arquivos *.cshtml* de itens de `Content` em itens de `RazorGenerate`.</span><span class="sxs-lookup"><span data-stu-id="a0d86-150">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| <span data-ttu-id="a0d86-151">GenerateRazorTargetAssemblyInfo</span><span class="sxs-lookup"><span data-stu-id="a0d86-151">GenerateRazorTargetAssemblyInfo</span></span>               | <span data-ttu-id="a0d86-152">Quando `true`, gera um arquivo *.cs* que contém atributos especificados pelo `RazorAssemblyAttribute` e os inclui na saída da compilação.</span><span class="sxs-lookup"><span data-stu-id="a0d86-152">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes it in the compile output.</span></span> |
| <span data-ttu-id="a0d86-153">EnableDefaultRazorTargetAssemblyInfoAttributes</span><span class="sxs-lookup"><span data-stu-id="a0d86-153">EnableDefaultRazorTargetAssemblyInfoAttributes</span></span> | <span data-ttu-id="a0d86-154">Quando `true`, adiciona um conjunto padrão de atributos de assembly em `RazorAssemblyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="a0d86-154">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| <span data-ttu-id="a0d86-155">CopyRazorGenerateFilesToPublishDirectory</span><span class="sxs-lookup"><span data-stu-id="a0d86-155">CopyRazorGenerateFilesToPublishDirectory</span></span>       | <span data-ttu-id="a0d86-156">Quando `true`, copia arquivos de itens de RazorGenerate (*.cshtml*) no diretório de publicação.</span><span class="sxs-lookup"><span data-stu-id="a0d86-156">When `true`, copies RazorGenerate items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="a0d86-157">Normalmente, os arquivos Razor não são necessários para um aplicativo publicado quando eles participam da compilação no tempo de build ou no tempo de publicação.</span><span class="sxs-lookup"><span data-stu-id="a0d86-157">Typically Razor files are not needed for a published application if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="a0d86-158">Assume o padrão de `false`.</span><span class="sxs-lookup"><span data-stu-id="a0d86-158">Defaults to `false`.</span></span> |
| <span data-ttu-id="a0d86-159">CopyRefAssembliesToPublishDirectory</span><span class="sxs-lookup"><span data-stu-id="a0d86-159">CopyRefAssembliesToPublishDirectory</span></span>            | <span data-ttu-id="a0d86-160">Quando `true`, copia os itens do assembly de referência no diretório de publicação.</span><span class="sxs-lookup"><span data-stu-id="a0d86-160">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="a0d86-161">Normalmente os assemblies de referência não são necessários para um aplicativo publicado quando a compilação do Razor ocorre no tempo de build ou no tempo de publicação.</span><span class="sxs-lookup"><span data-stu-id="a0d86-161">Typically reference assemblies are not needed for a published application if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="a0d86-162">Definido como `true`, por exemplo, se o aplicativo publicado requer a compilação no tempo de execução, ele modifica os arquivos cshtml no tempo de execução ou usa exibições inseridas.</span><span class="sxs-lookup"><span data-stu-id="a0d86-162">Set to `true`, if your published application requires runtime compilation, for example, modifies cshtml files at runtime, or uses embedded views.</span></span> <span data-ttu-id="a0d86-163">Assume o padrão de `false`.</span><span class="sxs-lookup"><span data-stu-id="a0d86-163">Defaults to `false`.</span></span> |
| <span data-ttu-id="a0d86-164">IncludeRazorContentInPack</span><span class="sxs-lookup"><span data-stu-id="a0d86-164">IncludeRazorContentInPack</span></span>                      | <span data-ttu-id="a0d86-165">Quando `true`, todos os itens de conteúdo do Razor (arquivos *.cshtml*) serão marcados para inclusão no pacote do NuGet gerado.</span><span class="sxs-lookup"><span data-stu-id="a0d86-165">When `true`, all Razor content items (*.cshtml* files) will be marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="a0d86-166">Assume o padrão de `false`.</span><span class="sxs-lookup"><span data-stu-id="a0d86-166">Defaults to `false`.</span></span> |
| <span data-ttu-id="a0d86-167">EmbedRazorGenerateSources</span><span class="sxs-lookup"><span data-stu-id="a0d86-167">EmbedRazorGenerateSources</span></span> | <span data-ttu-id="a0d86-168">Quando `true`, adiciona itens de RazorGenerate (*.cshtml*) como arquivos incorporados ao assembly Razor gerado.</span><span class="sxs-lookup"><span data-stu-id="a0d86-168">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="a0d86-169">Assume o padrão de `false`.</span><span class="sxs-lookup"><span data-stu-id="a0d86-169">Defaults to `false`.</span></span> |
| <span data-ttu-id="a0d86-170">UseRazorBuildServer</span><span class="sxs-lookup"><span data-stu-id="a0d86-170">UseRazorBuildServer</span></span>                           | <span data-ttu-id="a0d86-171">Quando `true`, usa um processo de servidor de build persistente para descarregar o trabalho de geração de código.</span><span class="sxs-lookup"><span data-stu-id="a0d86-171">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="a0d86-172">Seu valor padrão é `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="a0d86-172">Defaults to the value of `UseSharedCompilation`.</span></span> |

### <a name="targets"></a><span data-ttu-id="a0d86-173">Destinos</span><span class="sxs-lookup"><span data-stu-id="a0d86-173">Targets</span></span>
<span data-ttu-id="a0d86-174">O SDK do Razor define dois destinos primários:</span><span class="sxs-lookup"><span data-stu-id="a0d86-174">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="a0d86-175">`RazorGenerate` – o código gera arquivos *.cs* dos elementos de item de RazorGenerate.</span><span class="sxs-lookup"><span data-stu-id="a0d86-175">`RazorGenerate` - Code generates *.cs* files from RazorGenerate item elements.</span></span> <span data-ttu-id="a0d86-176">Use a propriedade `RazorGenerateDependsOn` para especificar destinos adicionais que podem ser executados antes ou depois desse destino.</span><span class="sxs-lookup"><span data-stu-id="a0d86-176">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="a0d86-177">`RazorCompile` – compila arquivos *.cs* gerados em um assembly Razor.</span><span class="sxs-lookup"><span data-stu-id="a0d86-177">`RazorCompile` - Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="a0d86-178">Use `RazorCompileDependsOn` para especificar destinos adicionais que podem ser executados antes ou depois desse destino.</span><span class="sxs-lookup"><span data-stu-id="a0d86-178">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>
---
title: Compilação e pré-compilação da exibição do Razor no ASP.NET Core
author: rick-anderson
description: Saiba como habilitar a compilação e a pré-compilação da exibição do MVC Razor nos aplicativos ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 5d971645106a79497a9902063c7774dc6d546395
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="415b7-103">Compilação e pré-compilação da exibição do Razor no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="415b7-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="415b7-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="415b7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="415b7-105">As exibições do Razor são compiladas em tempo de execução quando a exibição é invocada.</span><span class="sxs-lookup"><span data-stu-id="415b7-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="415b7-106">O ASP.NET Core 1.1.0 e superior pode opcionalmente compilar exibições do Razor e implantá-las com o aplicativo – um processo conhecido como pré-compilação.</span><span class="sxs-lookup"><span data-stu-id="415b7-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="415b7-107">Os modelos de projeto do ASP.NET Core 2.x habilitam a pré-compilação por padrão.</span><span class="sxs-lookup"><span data-stu-id="415b7-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="415b7-108">A pré-compilação da exibição do Razor não está disponível no momento durante a execução de uma [SCD (implantação autossuficiente)](/dotnet/core/deploying/#self-contained-deployments-scd) no ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="415b7-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="415b7-109">O recurso estará disponível para SCDs na versão 2.1.</span><span class="sxs-lookup"><span data-stu-id="415b7-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="415b7-110">Para obter mais informações, consulte [A compilação de exibição falha durante a compilação cruzada para Linux no Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span><span class="sxs-lookup"><span data-stu-id="415b7-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="415b7-111">Considerações sobre a pré-compilação:</span><span class="sxs-lookup"><span data-stu-id="415b7-111">Precompilation considerations:</span></span>

* <span data-ttu-id="415b7-112">As exibições de pré-compilação resultam em um menor pacote publicado e um tempo de inicialização mais rápido.</span><span class="sxs-lookup"><span data-stu-id="415b7-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="415b7-113">Não é possível editar arquivos do Razor depois que de pré-compilar exibições.</span><span class="sxs-lookup"><span data-stu-id="415b7-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="415b7-114">As exibições editadas não estarão presentes no pacote publicado.</span><span class="sxs-lookup"><span data-stu-id="415b7-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="415b7-115">Para implantar exibições pré-compiladas:</span><span class="sxs-lookup"><span data-stu-id="415b7-115">To deploy precompiled views:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="415b7-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="415b7-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="415b7-117">Se o projeto for direcionado ao .NET Framework, inclua uma referência de pacote a [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="415b7-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="415b7-118">Se o projeto for direcionado ao .NET Core, nenhuma alteração será necessária.</span><span class="sxs-lookup"><span data-stu-id="415b7-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="415b7-119">Os modelos de projeto do ASP.NET Core 2.x definem `MvcRazorCompileOnPublish` de forma implícita como `true` por padrão, o que significa que este nó pode ser removido com segurança do arquivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="415b7-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="415b7-120">Se você preferir que isso seja explícito, não há nenhum problema em configurar a propriedade `MvcRazorCompileOnPublish` como `true`.</span><span class="sxs-lookup"><span data-stu-id="415b7-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="415b7-121">A seguinte amostra *.csproj* realça essa configuração:</span><span class="sxs-lookup"><span data-stu-id="415b7-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=5)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="415b7-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="415b7-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="415b7-123">Defina `MvcRazorCompileOnPublish` como `true` e inclua uma referência de pacote a `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="415b7-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="415b7-124">A seguinte amostra *.csproj* realça essas configurações:</span><span class="sxs-lookup"><span data-stu-id="415b7-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

<span data-ttu-id="415b7-125">Prepare o aplicativo para uma [implantação dependente de estrutura](/dotnet/core/deploying/#framework-dependent-deployments-fdd) com o [comando de publicação da CLI do .NET Core](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="415b7-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="415b7-126">Por exemplo, execute o seguinte comando na raiz do projeto:</span><span class="sxs-lookup"><span data-stu-id="415b7-126">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="415b7-127">Um arquivo *<nome_do_projeto>.PrecompiledViews.dll*, que contém as exibições do Razor compiladas, é produzido quando a pré-compilação é bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="415b7-127">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="415b7-128">Por exemplo, a captura de tela abaixo mostra o conteúdo de *Index.cshtml* dentro de *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="415b7-128">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Exibições do Razor dentro da DLL](view-compilation/_static/razor-views-in-dll.png)

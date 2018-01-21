---
title: "Pré-compilação e a compilação de exibição do razor"
author: rick-anderson
description: "Um documento de referência que explicam como habilitar a compilação de exibição Razor do MVC e pré-compilação em aplicativos do ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 12/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 87989455c2fb6b5a922c7fb6133aa3e8cef42c88
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="7c0ed-103">Compilação de exibição Razor e pré-compilação no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7c0ed-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="7c0ed-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7c0ed-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7c0ed-105">Modos de exibição Razor são compilados em tempo de execução quando o modo de exibição é invocado.</span><span class="sxs-lookup"><span data-stu-id="7c0ed-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="7c0ed-106">ASP.NET Core 1.1.0 e superior pode opcionalmente compilar exibições Razor e implantá-las com o aplicativo&mdash;um processo conhecido como pré-compilação.</span><span class="sxs-lookup"><span data-stu-id="7c0ed-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="7c0ed-107">Os modelos de projeto do ASP.NET Core 2. x habilitam pré-compilação por padrão.</span><span class="sxs-lookup"><span data-stu-id="7c0ed-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7c0ed-108">Pré-compilação de exibição Razor está indisponível no momento ao executar uma [autossuficiente de implantação (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) no ASP.NET 2.0 de núcleo.</span><span class="sxs-lookup"><span data-stu-id="7c0ed-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="7c0ed-109">O recurso estará disponível para SCDs ao versões 2.1.</span><span class="sxs-lookup"><span data-stu-id="7c0ed-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="7c0ed-110">Para obter mais informações, consulte [compilação exibição falha durante a compilação cruzada para Linux no Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span><span class="sxs-lookup"><span data-stu-id="7c0ed-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="7c0ed-111">Considerações de pré-compilação:</span><span class="sxs-lookup"><span data-stu-id="7c0ed-111">Precompilation considerations:</span></span>

* <span data-ttu-id="7c0ed-112">Exibições de pré-compilação resulta em uma menor pacote publicado e o tempo de inicialização mais rápido.</span><span class="sxs-lookup"><span data-stu-id="7c0ed-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="7c0ed-113">Depois que você pré-compilar modos de exibição, você não pode editar arquivos do Razor.</span><span class="sxs-lookup"><span data-stu-id="7c0ed-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="7c0ed-114">Os modos de exibição editados não está presentes no pacote publicado.</span><span class="sxs-lookup"><span data-stu-id="7c0ed-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="7c0ed-115">Para implantar pré-compilado exibições:</span><span class="sxs-lookup"><span data-stu-id="7c0ed-115">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7c0ed-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7c0ed-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7c0ed-117">Se seu projeto se destina ao .NET Framework, incluir uma referência de pacote para [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="7c0ed-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="7c0ed-118">Se seu projeto se destina ao .NET Core, não são necessárias alterações.</span><span class="sxs-lookup"><span data-stu-id="7c0ed-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="7c0ed-119">Os modelos de projeto do ASP.NET Core 2. x definem implicitamente `MvcRazorCompileOnPublish` para `true` por padrão, que significa que este nó pode ser removido com segurança do *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="7c0ed-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="7c0ed-120">Se você preferir ser explícita, não há qualquer problema na configuração de `MvcRazorCompileOnPublish` propriedade `true`.</span><span class="sxs-lookup"><span data-stu-id="7c0ed-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="7c0ed-121">O seguinte *. csproj* exemplo destaca essa configuração:</span><span class="sxs-lookup"><span data-stu-id="7c0ed-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7c0ed-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7c0ed-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7c0ed-123">Definir `MvcRazorCompileOnPublish` para `true`e incluir uma referência de pacote para `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="7c0ed-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="7c0ed-124">O seguinte *. csproj* exemplo realça essas configurações:</span><span class="sxs-lookup"><span data-stu-id="7c0ed-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

<span data-ttu-id="7c0ed-125">Preparar o aplicativo para um [implantação dependentes de framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) executando um comando como o seguinte na raiz do projeto:</span><span class="sxs-lookup"><span data-stu-id="7c0ed-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) by executing a command such as the following at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="7c0ed-126">Um *< nome_do_projeto >. PrecompiledViews.dll* arquivo que contém os modos de exibição do Razor compilados é produzido quando pré-compilação for bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="7c0ed-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="7c0ed-127">Por exemplo, a captura de tela abaixo mostra o conteúdo de *cshtml* dentro de *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="7c0ed-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Modos de exibição Razor dentro de DLL](view-compilation/_static/razor-views-in-dll.png)

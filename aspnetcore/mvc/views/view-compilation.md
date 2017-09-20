---
title: "Pré-compilação e a compilação de exibição do razor"
author: rick-anderson
description: "Um documento de referência que explicam como habilitar a compilação de exibição Razor do MVC e pré-compilação em aplicativos do ASP.NET Core."
keywords: "ASP.NET Core, compilação de exibição Razor, Razor pré-compilação, pré-compilação do Razor"
ms.author: riande
manager: wpickett
ms.date: 08/16/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 0cb61315916d1b38f7cab3339e150c446fb69d98
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="8c35b-104">Compilação de exibição Razor e pré-compilação no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8c35b-104">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="8c35b-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8c35b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8c35b-106">Modos de exibição Razor são compilados em tempo de execução quando o modo de exibição é invocado.</span><span class="sxs-lookup"><span data-stu-id="8c35b-106">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="8c35b-107">ASP.NET Core 1.1.0 e superior pode opcionalmente compilar exibições Razor e implantá-las com o aplicativo &mdash; um processo conhecido como pré-compilação.</span><span class="sxs-lookup"><span data-stu-id="8c35b-107">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app &mdash; a process known as precompilation.</span></span> <span data-ttu-id="8c35b-108">Os modelos de projeto do ASP.NET Core 2. x habilitam pré-compilação por padrão.</span><span class="sxs-lookup"><span data-stu-id="8c35b-108">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!NOTE]
> <span data-ttu-id="8c35b-109">A pré-compilação de exibição Razor não está disponível ao fazer uma [Self-Contained implantação](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) em versões do ASP.NET Core 2.0.0 e versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="8c35b-109">Razor view precompilation is unavailable when doing a [Self-Contained Deployment](https://docs.microsoft.com/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core versions 2.0.0 and earlier.</span></span>

<span data-ttu-id="8c35b-110">Considerações de pré-compilação:</span><span class="sxs-lookup"><span data-stu-id="8c35b-110">Precompilation considerations:</span></span>

* <span data-ttu-id="8c35b-111">Exibições de pré-compilação resulta em uma menor pacote publicado e o tempo de inicialização mais rápido.</span><span class="sxs-lookup"><span data-stu-id="8c35b-111">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="8c35b-112">Depois que você pré-compilar modos de exibição, você não pode editar arquivos do Razor.</span><span class="sxs-lookup"><span data-stu-id="8c35b-112">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="8c35b-113">Os modos de exibição editados não está presentes no pacote publicado.</span><span class="sxs-lookup"><span data-stu-id="8c35b-113">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="8c35b-114">Para implantar pré-compilado exibições:</span><span class="sxs-lookup"><span data-stu-id="8c35b-114">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8c35b-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="8c35b-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8c35b-116">Se seu projeto se destina ao .NET Framework, incluir uma referência de pacote para `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:</span><span class="sxs-lookup"><span data-stu-id="8c35b-116">If your project targets .NET Framework, include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="8c35b-117">Se seu projeto se destina ao .NET Core, não são necessárias alterações.</span><span class="sxs-lookup"><span data-stu-id="8c35b-117">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="8c35b-118">Os modelos de projeto do ASP.NET Core 2. x definem implicitamente `MvcRazorCompileOnPublish` para `true` por padrão, que significa que este nó pode ser removido com segurança do *. csproj* arquivo.</span><span class="sxs-lookup"><span data-stu-id="8c35b-118">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="8c35b-119">Se você preferir ser explícita, não há qualquer problema na configuração de `MvcRazorCompileOnPublish` propriedade `true`.</span><span class="sxs-lookup"><span data-stu-id="8c35b-119">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="8c35b-120">O seguinte *. csproj* exemplo destaca essa configuração:</span><span class="sxs-lookup"><span data-stu-id="8c35b-120">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8c35b-121">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="8c35b-121">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8c35b-122">Definir `MvcRazorCompileOnPublish` para `true`e incluir uma referência de pacote para `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="8c35b-122">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="8c35b-123">O seguinte *. csproj* exemplo realça essas configurações:</span><span class="sxs-lookup"><span data-stu-id="8c35b-123">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

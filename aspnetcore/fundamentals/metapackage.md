---
title: Metapacote Microsoft.AspNetCore.All para ASP.NET Core 2.x e posterior
author: Rick-Anderson
description: O metapacote Microsoft.AspNetCore.All inclui todos os pacotes do ASP.NET Core e Entity Framework Core compatíveis, juntamente com suas dependências.
manager: wpickett
monikerRange: = aspnetcore-2.0
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 4c11f15e659565325bfe8b8d91188b62177b251d
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2018
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="dbfa3-103">Metapacote Microsoft.AspNetCore.All para ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="dbfa3-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="dbfa3-104">Este recurso exige o ASP.NET Core 2.x direcionado ao .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="dbfa3-105">O [Metapacote do Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core inclui:</span><span class="sxs-lookup"><span data-stu-id="dbfa3-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="dbfa3-106">Todos os pacotes com suporte da equipe do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="dbfa3-107">Todos os pacotes com suporte pelo Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="dbfa3-108">Dependências internas e de terceiros usadas por ASP.NET Core e pelo Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="dbfa3-109">Todos os recursos do ASP.NET Core 2.x e do Entity Framework Core 2.x são incluídos no pacote `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="dbfa3-110">Os modelos de projeto padrão direcionados para ASP.NET Core 2.0 usam este pacote.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-110">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="dbfa3-111">O número de versão do metapacote `Microsoft.AspNetCore.All` representa a versão do ASP.NET Core e a versão do Entity Framework Core (alinhadas com a versão do .NET Core).</span><span class="sxs-lookup"><span data-stu-id="dbfa3-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="dbfa3-112">Aplicativos que usam o metapacote `Microsoft.AspNetCore.All` aproveitam automaticamente o novo [Repositório de Tempo de Execução do .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="dbfa3-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="dbfa3-113">O Repositório de Tempo de Execução contém todos os ativos de tempo de execução necessários para executar aplicativos ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="dbfa3-114">Quando você usa o metapacote `Microsoft.AspNetCore.All`, **nenhum** ativo dos pacotes NuGet do ASP.NET Core referenciados é implantado com o aplicativo &mdash;, porque o Repositório de Tempo de Execução do .NET Core contém esse ativos.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="dbfa3-115">Os ativos no Repositório de Tempo de Execução são pré-compilados para melhorar o tempo de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="dbfa3-116">Use o processo de filtragem de pacote para remover os pacotes não utilizados.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="dbfa3-117">Pacotes filtrados são excluídos na saída do aplicativo publicado.</span><span class="sxs-lookup"><span data-stu-id="dbfa3-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="dbfa3-118">O seguinte arquivo *.csproj* referencia os metapacotes `Microsoft.AspNetCore.All` para o ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="dbfa3-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](../mvc/views/view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=9)]

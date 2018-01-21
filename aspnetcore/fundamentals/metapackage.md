---
title: Metapackage Microsoft.AspNetCore.All para ASP.NET Core 2. x e posterior
author: Rick-Anderson
description: "O metapackage Microsoft.AspNetCore.All inclui todos os pacotes do ASP.NET Core e o Entity Framework Core, juntamente com suas dependências."
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 8a44ee7ebb7e6b0112000429f1f080bceb7dc895
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="92ffc-103">Metapackage Microsoft.AspNetCore.All para ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="92ffc-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="92ffc-104">Este recurso requer .NET direcionamento do ASP.NET Core 2. x 2. x de núcleo.</span><span class="sxs-lookup"><span data-stu-id="92ffc-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="92ffc-105">O [Metapacote do Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core inclui:</span><span class="sxs-lookup"><span data-stu-id="92ffc-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="92ffc-106">Todos os pacotes com suporte da equipe do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="92ffc-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="92ffc-107">Todos os pacotes com suporte pelo Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="92ffc-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="92ffc-108">Dependências internas e de terceiros usadas por ASP.NET Core e pelo Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="92ffc-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="92ffc-109">Todos os recursos do ASP.NET Core 2. x e o Entity Framework Core 2. x são incluídos no `Microsoft.AspNetCore.All` pacote.</span><span class="sxs-lookup"><span data-stu-id="92ffc-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="92ffc-110">Os modelos de projeto padrão usam este pacote.</span><span class="sxs-lookup"><span data-stu-id="92ffc-110">The default project templates use this package.</span></span>

<span data-ttu-id="92ffc-111">O número de versão de `Microsoft.AspNetCore.All` metapackage representa a versão do ASP.NET Core e a versão do Entity Framework Core (alinhado com a versão do .NET Core).</span><span class="sxs-lookup"><span data-stu-id="92ffc-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="92ffc-112">Aplicativos que usam o `Microsoft.AspNetCore.All` metapackage automaticamente aproveitar o [repositório de tempo de execução do .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="92ffc-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="92ffc-113">O repositório de tempo de execução contém todos os ativos de tempo de execução necessários para executar o ASP.NET Core aplicativos 2. x.</span><span class="sxs-lookup"><span data-stu-id="92ffc-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="92ffc-114">Quando você usa o `Microsoft.AspNetCore.All` metapackage, **sem** ativos dos pacotes do NuGet do ASP.NET Core referenciados são implantados com o aplicativo &mdash; o repositório de tempo de execução do .NET Core contém esses ativos.</span><span class="sxs-lookup"><span data-stu-id="92ffc-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="92ffc-115">Os ativos no repositório de tempo de execução são pré-compilados para melhorar o tempo de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="92ffc-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="92ffc-116">Você pode usar o processo de remoção de pacote para remover os pacotes que não são usadas.</span><span class="sxs-lookup"><span data-stu-id="92ffc-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="92ffc-117">Pacotes cortados são excluídos na saída do aplicativo publicado.</span><span class="sxs-lookup"><span data-stu-id="92ffc-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="92ffc-118">O seguinte *. csproj* referências de arquivo a `Microsoft.AspNetCore.All` metapackage para ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="92ffc-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]

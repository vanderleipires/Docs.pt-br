---
title: Metapackage Microsoft.AspNetCore.All para ASP.NET Core 2. x e posterior
author: Rick-Anderson
description: "O metapackage Microsoft.AspNetCore.All inclui todos os pacotes do ASP.NET Core e o Entity Framework Core, juntamente com suas dependências."
keywords: Core,NuGet,package,Microsoft.AspNetCore.All,metapackage do ASP.NET
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: ff25d80be907994f7ac3d64a8ffa39ae53278ba6
ms.sourcegitcommit: 73bf6b222474d9f1f6aba3feaca4e191069d2121
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="618e4-104">Metapackage Microsoft.AspNetCore.All para ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="618e4-104">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="618e4-105">Este recurso requer .NET direcionamento do ASP.NET Core 2. x 2. x de núcleo.</span><span class="sxs-lookup"><span data-stu-id="618e4-105">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="618e4-106">O [Metapacote do Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core inclui:</span><span class="sxs-lookup"><span data-stu-id="618e4-106">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="618e4-107">Todos os pacotes com suporte da equipe do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="618e4-107">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="618e4-108">Todos os pacotes com suporte pelo Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="618e4-108">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="618e4-109">Dependências internas e de terceiros usadas por ASP.NET Core e pelo Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="618e4-109">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="618e4-110">Todos os recursos do ASP.NET Core 2. x e o Entity Framework Core 2. x são incluídos no `Microsoft.AspNetCore.All` pacote.</span><span class="sxs-lookup"><span data-stu-id="618e4-110">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="618e4-111">Os modelos de projeto padrão usam este pacote.</span><span class="sxs-lookup"><span data-stu-id="618e4-111">The default project templates use this package.</span></span>

<span data-ttu-id="618e4-112">O número de versão de `Microsoft.AspNetCore.All` metapackage representa a versão do ASP.NET Core e a versão do Entity Framework Core (alinhado com a versão do .NET Core).</span><span class="sxs-lookup"><span data-stu-id="618e4-112">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="618e4-113">Aplicativos que usam o `Microsoft.AspNetCore.All` metapackage automaticamente aproveitar o [repositório de tempo de execução do .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="618e4-113">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="618e4-114">O repositório de tempo de execução contém todos os ativos de tempo de execução necessários para executar o ASP.NET Core aplicativos 2. x.</span><span class="sxs-lookup"><span data-stu-id="618e4-114">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="618e4-115">Quando você usa o `Microsoft.AspNetCore.All` metapackage, **sem** ativos dos pacotes do NuGet do ASP.NET Core referenciados são implantados com o aplicativo &mdash; o repositório de tempo de execução do .NET Core contém esses ativos.</span><span class="sxs-lookup"><span data-stu-id="618e4-115">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="618e4-116">Os ativos no repositório de tempo de execução são pré-compilados para melhorar o tempo de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="618e4-116">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="618e4-117">Você pode usar o processo de remoção de pacote para remover os pacotes que não são usadas.</span><span class="sxs-lookup"><span data-stu-id="618e4-117">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="618e4-118">Pacotes cortados são excluídos na saída do aplicativo publicado.</span><span class="sxs-lookup"><span data-stu-id="618e4-118">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="618e4-119">O seguinte *. csproj* referências de arquivo a `Microsoft.AspNetCore.All` metapackage para ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="618e4-119">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]

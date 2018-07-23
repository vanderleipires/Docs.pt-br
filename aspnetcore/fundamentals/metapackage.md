---
title: Metapacote Microsoft.AspNetCore.All para ASP.NET Core 2.0
author: Rick-Anderson
description: O metapacote Microsoft.AspNetCore.All inclui todos os pacotes do ASP.NET Core e Entity Framework Core compatíveis, juntamente com suas dependências.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage
ms.openlocfilehash: fbc0f5465dc37a612b81c293f1a58b53ea4b2238
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123821"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="606cc-103">Metapacote Microsoft.AspNetCore.All para ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="606cc-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="606cc-104">Recomendamos que os aplicativos voltados para o ASP.NET Core 2.1 e posterior usem o [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) em vez deste pacote.</span><span class="sxs-lookup"><span data-stu-id="606cc-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="606cc-105">Veja [Migração do Microsoft.AspNetCore.All para Microsoft.AspNetCore.App](#migrate) neste artigo.</span><span class="sxs-lookup"><span data-stu-id="606cc-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="606cc-106">Este recurso exige o ASP.NET Core 2.x direcionado ao .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="606cc-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="606cc-107">O [Metapacote do Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) para ASP.NET Core inclui:</span><span class="sxs-lookup"><span data-stu-id="606cc-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="606cc-108">Todos os pacotes com suporte da equipe do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="606cc-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="606cc-109">Todos os pacotes com suporte pelo Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="606cc-109">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="606cc-110">Dependências internas e de terceiros usadas por ASP.NET Core e pelo Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="606cc-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="606cc-111">Todos os recursos do ASP.NET Core 2.x e do Entity Framework Core 2.x são incluídos no pacote `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="606cc-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="606cc-112">Os modelos de projeto padrão direcionados para ASP.NET Core 2.0 usam este pacote.</span><span class="sxs-lookup"><span data-stu-id="606cc-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="606cc-113">O número de versão do metapacote `Microsoft.AspNetCore.All` representa a versão do ASP.NET Core e a versão do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="606cc-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="606cc-114">Aplicativos que usam o metapacote `Microsoft.AspNetCore.All` aproveitam automaticamente o novo [Repositório de Tempo de Execução do .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="606cc-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="606cc-115">O Repositório de Tempo de Execução contém todos os ativos de tempo de execução necessários para executar aplicativos ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="606cc-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="606cc-116">Quando você usa o metapacote `Microsoft.AspNetCore.All`, **nenhum** ativo dos pacotes NuGet do ASP.NET Core referenciados é implantado com o aplicativo &mdash;, porque o Repositório de Tempo de Execução do .NET Core contém esse ativos.</span><span class="sxs-lookup"><span data-stu-id="606cc-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="606cc-117">Os ativos no Repositório de Tempo de Execução são pré-compilados para melhorar o tempo de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="606cc-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="606cc-118">Use o processo de filtragem de pacote para remover os pacotes não utilizados.</span><span class="sxs-lookup"><span data-stu-id="606cc-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="606cc-119">Pacotes filtrados são excluídos na saída do aplicativo publicado.</span><span class="sxs-lookup"><span data-stu-id="606cc-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="606cc-120">O seguinte arquivo *.csproj* referencia os metapacotes `Microsoft.AspNetCore.All` para o ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="606cc-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="606cc-121">Migração do Microsoft.AspNetCore.All para Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="606cc-121">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="606cc-122">Os pacotes a seguir estão incluídos no `Microsoft.AspNetCore.All`, mas não no pacote `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="606cc-122">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span> 

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

<span data-ttu-id="606cc-123">Para mover de `Microsoft.AspNetCore.All` para `Microsoft.AspNetCore.App`, se seu aplicativo usa APIs dos pacotes acima ou de pacotes trazidos por eles, adicione referências a eles em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="606cc-123">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="606cc-124">Todas as dependências dos pacotes anteriores que, de outra forma, não são dependências de `Microsoft.AspNetCore.App` não são incluídas implicitamente.</span><span class="sxs-lookup"><span data-stu-id="606cc-124">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="606cc-125">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="606cc-125">For example:</span></span>

* <span data-ttu-id="606cc-126">`StackExchange.Redis` como uma dependência de `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="606cc-126">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="606cc-127">`Microsoft.ApplicationInsights` como uma dependência de `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="606cc-127">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>

---
title: Solucionar problemas em projetos do ASP.NET Core
author: Rick-Anderson
description: Compreenda e solucione problemas de avisos e erros com projetos do ASP.NET Core.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: ae4e6f191d8f856de60ecf21cb882b5ee9b02064
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274587"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="56933-103">Solucionar problemas em projetos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56933-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="56933-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="56933-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="56933-105">Os links a seguir fornecem orientação para solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="56933-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="56933-106">Solucionar problemas no ASP.NET Core no Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="56933-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="56933-107">Solucionar problemas do ASP.NET Core no IIS</span><span class="sxs-lookup"><span data-stu-id="56933-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="56933-108">Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56933-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="56933-109">Conferência NDC (Londres, 2018): Diagnosticar problemas em aplicativos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56933-109">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="56933-110">Blog do ASP.NET: Solução de problemas de desempenho do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56933-110">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="56933-111">Avisos do SDK do .NET core</span><span class="sxs-lookup"><span data-stu-id="56933-111">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="56933-112">Os 32 bits e versões de 64 bits do SDK .NET Core estão instaladas</span><span class="sxs-lookup"><span data-stu-id="56933-112">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="56933-113">No **novo projeto** caixa de diálogo para o ASP.NET Core, você poderá ver o seguinte aviso:</span><span class="sxs-lookup"><span data-stu-id="56933-113">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="56933-114">Versões de bits de 32 e 64 do SDK .NET Core são instaladas.</span><span class="sxs-lookup"><span data-stu-id="56933-114">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="56933-115">Somente modelos das versões de 64 bits instalado em ' c:\\arquivos de programa\\dotnet\\sdk\\' será exibida.</span><span class="sxs-lookup"><span data-stu-id="56933-115">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Uma captura de tela da caixa de diálogo OneASP.NET mostrando a mensagem de aviso](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="56933-117">Esse aviso aparece quando (x86) 32 bits e versões de 64 bits (x64) da [.NET Core SDK](https://www.microsoft.com/net/download/all) estão instalados.</span><span class="sxs-lookup"><span data-stu-id="56933-117">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="56933-118">Ambas as versões podem ser instaladas os motivos comuns incluem:</span><span class="sxs-lookup"><span data-stu-id="56933-118">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="56933-119">Você originalmente baixou o instalador do SDK do .NET Core usando um computador de 32 bits, mas, em seguida, copiado entre e instalado em um computador de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="56933-119">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="56933-120">O SDK de núcleo do .NET 32 bits foi instalado por outro aplicativo.</span><span class="sxs-lookup"><span data-stu-id="56933-120">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="56933-121">A versão incorreta foi baixada e instalada.</span><span class="sxs-lookup"><span data-stu-id="56933-121">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="56933-122">Desinstale o SDK de núcleo do .NET 32 bits para evitar este aviso.</span><span class="sxs-lookup"><span data-stu-id="56933-122">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="56933-123">Desinstalar do **painel de controle** > **programas e recursos** > **desinstalar ou alterar um programa**.</span><span class="sxs-lookup"><span data-stu-id="56933-123">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="56933-124">Se você entender por que o aviso é exibido e suas implicações, você pode ignorar o aviso.</span><span class="sxs-lookup"><span data-stu-id="56933-124">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="56933-125">O SDK do .NET Core é instalado em vários locais</span><span class="sxs-lookup"><span data-stu-id="56933-125">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="56933-126">No **novo projeto** caixa de diálogo para o ASP.NET Core, você poderá ver o seguinte aviso:</span><span class="sxs-lookup"><span data-stu-id="56933-126">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="56933-127">O SDK do .NET Core é instalado em vários locais.</span><span class="sxs-lookup"><span data-stu-id="56933-127">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="56933-128">Somente modelos do SDK(s) instalado no ' c:\\arquivos de programa\\dotnet\\sdk\\' será exibida.</span><span class="sxs-lookup"><span data-stu-id="56933-128">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Uma captura de tela da caixa de diálogo OneASP.NET mostrando a mensagem de aviso](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="56933-130">Você verá esta mensagem quando você tem pelo menos uma instalação do SDK .NET Core em um diretório fora do *c:\\arquivos de programa\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="56933-130">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="56933-131">Normalmente, isso ocorre quando o SDK do .NET Core foi implantado em um computador usando copiar/colar em vez do instalador MSI.</span><span class="sxs-lookup"><span data-stu-id="56933-131">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="56933-132">Desinstale o SDK de núcleo do .NET 32 bits para evitar este aviso.</span><span class="sxs-lookup"><span data-stu-id="56933-132">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="56933-133">Desinstalar do **painel de controle** > **programas e recursos** > **desinstalar ou alterar um programa**.</span><span class="sxs-lookup"><span data-stu-id="56933-133">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="56933-134">Se você entender por que o aviso é exibido e suas implicações, você pode ignorar o aviso.</span><span class="sxs-lookup"><span data-stu-id="56933-134">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="56933-135">Nenhum SDK do .NET Core foram detectado</span><span class="sxs-lookup"><span data-stu-id="56933-135">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="56933-136">No **novo projeto** caixa de diálogo para o ASP.NET Core, você poderá ver o seguinte aviso:</span><span class="sxs-lookup"><span data-stu-id="56933-136">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="56933-137">Nenhum SDK do .NET Core foram detectado, certifique-se de que eles são incluídos na variável de ambiente 'PATH'.</span><span class="sxs-lookup"><span data-stu-id="56933-137">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Uma captura de tela da caixa de diálogo OneASP.NET mostrando a mensagem de aviso](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="56933-139">Esse aviso aparece quando a variável de ambiente `PATH` não aponta para qualquer SDKs de núcleo do .NET no computador.</span><span class="sxs-lookup"><span data-stu-id="56933-139">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="56933-140">Para resolver esse problema:</span><span class="sxs-lookup"><span data-stu-id="56933-140">To resolve this problem:</span></span>

* <span data-ttu-id="56933-141">Instale ou verifique se que o SDK de núcleo do .NET está instalado.</span><span class="sxs-lookup"><span data-stu-id="56933-141">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="56933-142">Verifique se o `PATH` variável de ambiente aponta para o local do SDK está instalado.</span><span class="sxs-lookup"><span data-stu-id="56933-142">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="56933-143">O instalador normalmente define o `PATH`.</span><span class="sxs-lookup"><span data-stu-id="56933-143">The installer normally sets the `PATH`.</span></span>

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a><span data-ttu-id="56933-144">Uso de IHtmlHelper.Partial pode resultar em deadlocks de aplicativo</span><span class="sxs-lookup"><span data-stu-id="56933-144">Use of IHtmlHelper.Partial may result in app deadlocks</span></span>

<span data-ttu-id="56933-145">No ASP.NET Core 2.1 e posterior, chamando `Html.Partial` resulta em um aviso de analisador devido ao potencial para deadlocks.</span><span class="sxs-lookup"><span data-stu-id="56933-145">In ASP.NET Core 2.1 and later, calling `Html.Partial` results in an analyzer warning due to the potential for deadlocks.</span></span> <span data-ttu-id="56933-146">A mensagem de aviso é:</span><span class="sxs-lookup"><span data-stu-id="56933-146">The warning message is:</span></span>

> <span data-ttu-id="56933-147">Uso de IHtmlHelper.Partial pode resultar em deadlocks de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="56933-147">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="56933-148">Considere o uso de `<partial>` auxiliar de marca ou `IHtmlHelper.PartialAsync`.</span><span class="sxs-lookup"><span data-stu-id="56933-148">Consider using `<partial>` Tag Helper or `IHtmlHelper.PartialAsync`.</span></span>

<span data-ttu-id="56933-149">Chamadas para `@Html.Partial` devem ser substituídos por `@await Html.PartialAsync` ou o auxiliar de marca parcial `<partial name="_Partial" />`.</span><span class="sxs-lookup"><span data-stu-id="56933-149">Calls to `@Html.Partial` should be replaced by `@await Html.PartialAsync` or the partial tag helper `<partial name="_Partial" />`.</span></span>

::: moniker-end

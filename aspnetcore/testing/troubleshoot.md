---
title: Solucionar problemas para o ASP.NET Core
author: Rick-Anderson
description: Compreender e solucionar problemas de avisos e erros com projetos do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: f2c785bfe27ddd67db0313b8ee1c077a8cc06e05
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="321b2-103">Solucionar problemas em projetos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="321b2-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="321b2-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="321b2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="321b2-105">Os links a seguir fornecem orientação para solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="321b2-105">The following links provide troubleshooting guidance:</span></span>

* [<span data-ttu-id="321b2-106">Solucionar problemas no ASP.NET Core no Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="321b2-106">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)
* [<span data-ttu-id="321b2-107">Solucionar problemas do ASP.NET Core no IIS</span><span class="sxs-lookup"><span data-stu-id="321b2-107">Troubleshoot ASP.NET Core on IIS</span></span>](xref:host-and-deploy/iis/troubleshoot)
* [<span data-ttu-id="321b2-108">Referência de erros comuns para o Serviço de Aplicativo do Azure e o IIS com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="321b2-108">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)
* [<span data-ttu-id="321b2-109">YouTube: diagnosticar problemas em aplicativos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="321b2-109">YouTube: Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a><span data-ttu-id="321b2-110">Avisos do SDK do .NET core</span><span class="sxs-lookup"><span data-stu-id="321b2-110">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="321b2-111">Os 32 bits e versões de 64 bits do SDK .NET Core estão instaladas</span><span class="sxs-lookup"><span data-stu-id="321b2-111">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>
<span data-ttu-id="321b2-112">No **novo projeto** caixa de diálogo para o ASP.NET Core, você poderá ver o seguinte aviso:</span><span class="sxs-lookup"><span data-stu-id="321b2-112">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span> 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Uma captura de tela da caixa de diálogo OneASP.NET mostrando a mensagem de aviso](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="321b2-114">Esse aviso aparece quando (x86) 32 bits e versões de 64 bits (x64) da [.NET Core SDK](https://www.microsoft.com/net/download/all) estão instalados.</span><span class="sxs-lookup"><span data-stu-id="321b2-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="321b2-115">Ambas as versões podem ser instaladas os motivos comuns incluem:</span><span class="sxs-lookup"><span data-stu-id="321b2-115">Common reasons both versions can be installed include:</span></span>

* <span data-ttu-id="321b2-116">Você baixou o instalador do SDK do .NET Core usando um computador de 32 bits, mas, em seguida, foi copiado em e originalmente instalado em um computador de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="321b2-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine, but then copied it across and installed it on a 64-bit machine.</span></span> 
* <span data-ttu-id="321b2-117">O SDK de núcleo do .NET 32 bits foi instalado por outro aplicativo.</span><span class="sxs-lookup"><span data-stu-id="321b2-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="321b2-118">A versão incorreta foi baixada e instalada.</span><span class="sxs-lookup"><span data-stu-id="321b2-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="321b2-119">Desinstale o SDK de núcleo do .NET 32 bits para evitar este aviso.</span><span class="sxs-lookup"><span data-stu-id="321b2-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="321b2-120">Desinstalar do **painel de controle** > **programas e recursos** > **desinstalar ou alterar um programa**.</span><span class="sxs-lookup"><span data-stu-id="321b2-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="321b2-121">Se você entender por que o aviso é exibido e suas implicações, você pode ignorar o aviso.</span><span class="sxs-lookup"><span data-stu-id="321b2-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="321b2-122">O SDK do .NET Core é instalado em vários locais</span><span class="sxs-lookup"><span data-stu-id="321b2-122">The .NET Core SDK is installed in multiple locations</span></span>
<span data-ttu-id="321b2-123">No **novo projeto** caixa de diálogo para o ASP.NET Core, você poderá ver o seguinte aviso:</span><span class="sxs-lookup"><span data-stu-id="321b2-123">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

 <span data-ttu-id="321b2-124">O SDK do .NET Core é instalado em vários locais.</span><span class="sxs-lookup"><span data-stu-id="321b2-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="321b2-125">Somente modelos a partir de SDK(s) instalado no ' C:\Program Files\dotnet\sdk\' será exibido.</span><span class="sxs-lookup"><span data-stu-id="321b2-125">Only templates from the SDK(s) installed at 'C:\Program Files\dotnet\sdk\' will be displayed.</span></span>

![Uma captura de tela da caixa de diálogo OneASP.NET mostrando a mensagem de aviso](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="321b2-127">Você verá esta mensagem quando você tem pelo menos uma instalação do SDK .NET Core em um diretório fora do * C:\Program Files\dotnet\sdk\*.</span><span class="sxs-lookup"><span data-stu-id="321b2-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\Program Files\dotnet\sdk\*.</span></span> <span data-ttu-id="321b2-128">Geralmente, isso acontece quando o SDK do .NET Core foi implantado em um computador usando copiar/colar em vez do instalador MSI.</span><span class="sxs-lookup"><span data-stu-id="321b2-128">Usually that happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="321b2-129">Desinstale o SDK de núcleo do .NET 32 bits para evitar este aviso.</span><span class="sxs-lookup"><span data-stu-id="321b2-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="321b2-130">Desinstalar do **painel de controle** > **programas e recursos** > **desinstalar ou alterar um programa**.</span><span class="sxs-lookup"><span data-stu-id="321b2-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="321b2-131">Se você entender por que o aviso é exibido e suas implicações, você pode ignorar o aviso.</span><span class="sxs-lookup"><span data-stu-id="321b2-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="321b2-132">Nenhum SDK do .NET Core foram detectado</span><span class="sxs-lookup"><span data-stu-id="321b2-132">No .NET Core SDKs were detected</span></span>
<span data-ttu-id="321b2-133">No **novo projeto** caixa de diálogo para o ASP.NET Core, você poderá ver o seguinte aviso:</span><span class="sxs-lookup"><span data-stu-id="321b2-133">In the **New Project** dialog for ASP.NET Core you may see the following warning:</span></span> 

<span data-ttu-id="321b2-134">**Nenhum SDK do .NET Core foram detectado, certifique-se de que eles são incluídos na variável de ambiente 'PATH'**</span><span class="sxs-lookup"><span data-stu-id="321b2-134">**No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'**</span></span>

![Uma captura de tela da caixa de diálogo OneASP.NET mostrando a mensagem de aviso](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="321b2-136">Esse aviso aparece quando a variável de ambiente `PATH` não aponta para qualquer SDKs de núcleo do .NET no computador.</span><span class="sxs-lookup"><span data-stu-id="321b2-136">This warning appears when the environment variable `PATH` doesn’t point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="321b2-137">Para resolver esse problema:</span><span class="sxs-lookup"><span data-stu-id="321b2-137">To resolve this problem:</span></span>

* <span data-ttu-id="321b2-138">Instale ou verifique se que o SDK de núcleo do .NET está instalado.</span><span class="sxs-lookup"><span data-stu-id="321b2-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="321b2-139">Verifique se o `PATH` variável de ambiente aponta para o local do SDK está instalado.</span><span class="sxs-lookup"><span data-stu-id="321b2-139">Verify the `PATH` environment variable points to the location the SDK is installed.</span></span> <span data-ttu-id="321b2-140">O instalador normalmente define o `PATH`.</span><span class="sxs-lookup"><span data-stu-id="321b2-140">The installer normally sets the `PATH`.</span></span>
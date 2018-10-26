---
title: Solucionar problemas de projetos do ASP.NET Core
author: Rick-Anderson
description: Compreenda e solucione problemas de avisos e erros com projetos do ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: test/troubleshoot
ms.openlocfilehash: 150f2192bb4b6dd0d330fd678d9c5fa0bf31673e
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090105"
---
# <a name="troubleshoot-aspnet-core-projects"></a><span data-ttu-id="715ba-103">Solucionar problemas de projetos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="715ba-103">Troubleshoot ASP.NET Core projects</span></span>

<span data-ttu-id="715ba-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="715ba-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="715ba-105">Os links a seguir fornecem diretrizes de solução de problemas:</span><span class="sxs-lookup"><span data-stu-id="715ba-105">The following links provide troubleshooting guidance:</span></span>

* <xref:host-and-deploy/azure-apps/troubleshoot>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-iis-errors-reference>
* [<span data-ttu-id="715ba-106">NDC Conference (2018 Londres): Diagnosticar problemas em aplicativos do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="715ba-106">NDC Conference (London, 2018): Diagnosing issues in ASP.NET Core Applications</span></span>](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [<span data-ttu-id="715ba-107">Blog do ASP.NET: Solucionando problemas de desempenho do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="715ba-107">ASP.NET Blog: Troubleshooting ASP.NET Core Performance Problems</span></span>](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a><span data-ttu-id="715ba-108">Avisos do SDK do .NET core</span><span class="sxs-lookup"><span data-stu-id="715ba-108">.NET Core SDK warnings</span></span>

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a><span data-ttu-id="715ba-109">Os 32 bits e versões de 64 bits do SDK do .NET Core estão instaladas</span><span class="sxs-lookup"><span data-stu-id="715ba-109">Both the 32 bit and 64 bit versions of the .NET Core SDK are installed</span></span>

<span data-ttu-id="715ba-110">No **novo projeto** caixa de diálogo para o ASP.NET Core, você poderá ver o seguinte aviso:</span><span class="sxs-lookup"><span data-stu-id="715ba-110">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="715ba-111">As versões de bit 32 e 64 do SDK do .NET Core são instaladas.</span><span class="sxs-lookup"><span data-stu-id="715ba-111">Both 32 and 64 bit versions of the .NET Core SDK are installed.</span></span> <span data-ttu-id="715ba-112">Somente modelos das versões de 64 bits instalados em ' c:\\Program Files\\dotnet\\sdk\\' será exibida.</span><span class="sxs-lookup"><span data-stu-id="715ba-112">Only templates from the 64 bit version(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Uma captura de tela da caixa de diálogo OneASP.NET mostrando a mensagem de aviso](troubleshoot/_static/both32and64bit.png)

<span data-ttu-id="715ba-114">Esse aviso é exibido quando (x86) 32 bits e 64 bits (x64) versões dos [SDK do .NET Core](https://www.microsoft.com/net/download/all) estão instalados.</span><span class="sxs-lookup"><span data-stu-id="715ba-114">This warning appears when both 32-bit (x86) and 64-bit (x64) versions of the [.NET Core SDK](https://www.microsoft.com/net/download/all) are installed.</span></span> <span data-ttu-id="715ba-115">Ambas as versões podem ser instaladas os motivos comuns incluem:</span><span class="sxs-lookup"><span data-stu-id="715ba-115">Common reasons both versions may be installed include:</span></span>

* <span data-ttu-id="715ba-116">Você originalmente baixou o instalador do SDK do .NET Core usando um computador de 32 bits, mas, em seguida, copiado entre e instalado em um computador de 64 bits.</span><span class="sxs-lookup"><span data-stu-id="715ba-116">You originally downloaded the .NET Core SDK installer using a 32-bit machine but then copied it across and installed it on a 64-bit machine.</span></span>
* <span data-ttu-id="715ba-117">O .NET Core SDK de 32 bits foi instalado por outro aplicativo.</span><span class="sxs-lookup"><span data-stu-id="715ba-117">The 32-bit .NET Core SDK was installed by another application.</span></span>
* <span data-ttu-id="715ba-118">A versão incorreta foi baixada e instalada.</span><span class="sxs-lookup"><span data-stu-id="715ba-118">The wrong version was downloaded and installed.</span></span>

<span data-ttu-id="715ba-119">Desinstale o .NET Core SDK 32 bits para evitar esse aviso.</span><span class="sxs-lookup"><span data-stu-id="715ba-119">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="715ba-120">Desinstalar do **painel de controle** > **programas e recursos** > **desinstalar ou alterar um programa**.</span><span class="sxs-lookup"><span data-stu-id="715ba-120">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="715ba-121">Se você entender por que o aviso ocorre e suas implicações, você pode ignorar o aviso.</span><span class="sxs-lookup"><span data-stu-id="715ba-121">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a><span data-ttu-id="715ba-122">SDK do .NET Core é instalado em vários locais</span><span class="sxs-lookup"><span data-stu-id="715ba-122">The .NET Core SDK is installed in multiple locations</span></span>

<span data-ttu-id="715ba-123">No **novo projeto** caixa de diálogo para o ASP.NET Core, você poderá ver o seguinte aviso:</span><span class="sxs-lookup"><span data-stu-id="715ba-123">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="715ba-124">SDK do .NET Core é instalado em vários locais.</span><span class="sxs-lookup"><span data-stu-id="715ba-124">The .NET Core SDK is installed in multiple locations.</span></span> <span data-ttu-id="715ba-125">Somente modelos dos SDKs instalados em ' c:\\Program Files\\dotnet\\sdk\\' será exibida.</span><span class="sxs-lookup"><span data-stu-id="715ba-125">Only templates from the SDK(s) installed at 'C:\\Program Files\\dotnet\\sdk\\' will be displayed.</span></span>

![Uma captura de tela da caixa de diálogo OneASP.NET mostrando a mensagem de aviso](troubleshoot/_static/multiplelocations.png)

<span data-ttu-id="715ba-127">Você verá esta mensagem quando você tem pelo menos uma instalação do SDK do .NET Core em um diretório fora do *c:\\arquivos de programas\\dotnet\\sdk\\*.</span><span class="sxs-lookup"><span data-stu-id="715ba-127">You see this message when you have at least one installation of the .NET Core SDK in a directory outside of *C:\\Program Files\\dotnet\\sdk\\*.</span></span> <span data-ttu-id="715ba-128">Geralmente isso acontece quando o SDK do .NET Core foi implantado em um computador usando copiar/colar em vez do instalador MSI.</span><span class="sxs-lookup"><span data-stu-id="715ba-128">Usually this happens when the .NET Core SDK has been deployed on a machine using copy/paste instead of the MSI installer.</span></span>

<span data-ttu-id="715ba-129">Desinstale o .NET Core SDK 32 bits para evitar esse aviso.</span><span class="sxs-lookup"><span data-stu-id="715ba-129">Uninstall the 32-bit .NET Core SDK to prevent this warning.</span></span> <span data-ttu-id="715ba-130">Desinstalar do **painel de controle** > **programas e recursos** > **desinstalar ou alterar um programa**.</span><span class="sxs-lookup"><span data-stu-id="715ba-130">Uninstall from **Control Panel** > **Programs and Features** > **Uninstall or change a program**.</span></span> <span data-ttu-id="715ba-131">Se você entender por que o aviso ocorre e suas implicações, você pode ignorar o aviso.</span><span class="sxs-lookup"><span data-stu-id="715ba-131">If you understand why the warning occurs and its implications, you can ignore the warning.</span></span>

### <a name="no-net-core-sdks-were-detected"></a><span data-ttu-id="715ba-132">Não há SDKs do .NET Core foram detectados</span><span class="sxs-lookup"><span data-stu-id="715ba-132">No .NET Core SDKs were detected</span></span>

<span data-ttu-id="715ba-133">No **novo projeto** caixa de diálogo para o ASP.NET Core, você poderá ver o seguinte aviso:</span><span class="sxs-lookup"><span data-stu-id="715ba-133">In the **New Project** dialog for ASP.NET Core, you may see the following warning:</span></span>

> <span data-ttu-id="715ba-134">Nenhum SDK do .NET Core foi detectado, verifique se que eles estão incluídos na variável de ambiente 'PATH'.</span><span class="sxs-lookup"><span data-stu-id="715ba-134">No .NET Core SDKs were detected, ensure they are included in the environment variable 'PATH'.</span></span>

![Uma captura de tela da caixa de diálogo OneASP.NET mostrando a mensagem de aviso](troubleshoot/_static/NoNetCore.png)

<span data-ttu-id="715ba-136">Esse aviso é exibido quando a variável de ambiente `PATH` não apontar para qualquer SDKs do .NET Core no computador.</span><span class="sxs-lookup"><span data-stu-id="715ba-136">This warning appears when the environment variable `PATH` doesn't point to any .NET Core SDKs on the machine.</span></span> <span data-ttu-id="715ba-137">Para resolver esse problema:</span><span class="sxs-lookup"><span data-stu-id="715ba-137">To resolve this problem:</span></span>

* <span data-ttu-id="715ba-138">Instale ou verifique se o que SDK do .NET Core é instalado.</span><span class="sxs-lookup"><span data-stu-id="715ba-138">Install or verify the .NET Core SDK is installed.</span></span>
* <span data-ttu-id="715ba-139">Verifique o `PATH` variável de ambiente aponta para o local em que o SDK está instalado.</span><span class="sxs-lookup"><span data-stu-id="715ba-139">Verify that the `PATH` environment variable points to the location in which the SDK is installed.</span></span> <span data-ttu-id="715ba-140">O instalador normalmente define o `PATH`.</span><span class="sxs-lookup"><span data-stu-id="715ba-140">The installer normally sets the `PATH`.</span></span>

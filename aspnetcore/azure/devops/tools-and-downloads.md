---
title: DevOps com o ASP.NET Core e o Azure | Ferramentas e downloads
author: CamSoper
description: Um guia que fornece orientação de ponta a ponta sobre a criação de um pipeline de DevOps para um aplicativo ASP.NET Core hospedado no Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/tools-and-downloads
ms.openlocfilehash: 5529068b83db475315784571fbf4151d7ecd0d5d
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340154"
---
# <a name="tools-and-downloads"></a><span data-ttu-id="ee37a-103">Ferramentas e downloads</span><span class="sxs-lookup"><span data-stu-id="ee37a-103">Tools and downloads</span></span>

<span data-ttu-id="ee37a-104">O Azure tem várias interfaces para provisionamento e gerenciamento de recursos, como o [portal do Azure](https://portal.azure.com), [CLI do Azure](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [nuvem do Azure Shell](https://shell.azure.com/bash)e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ee37a-104">Azure has several interfaces for provisioning and managing resources, such as the [Azure portal](https://portal.azure.com), [Azure CLI](https://docs.microsoft.com/cli/azure/), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview), [Azure Cloud Shell](https://shell.azure.com/bash), and Visual Studio.</span></span> <span data-ttu-id="ee37a-105">Este guia usa uma abordagem minimalista e usa o Azure Cloud Shell sempre que possível para reduzir as etapas necessárias.</span><span class="sxs-lookup"><span data-stu-id="ee37a-105">This guide takes a minimalist approach and uses the Azure Cloud Shell whenever possible to reduce the steps required.</span></span> <span data-ttu-id="ee37a-106">No entanto, o portal do Azure deve ser usado para algumas partes.</span><span class="sxs-lookup"><span data-stu-id="ee37a-106">However, the Azure portal must be used for some portions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ee37a-107">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="ee37a-107">Prerequisites</span></span>

<span data-ttu-id="ee37a-108">As assinaturas a seguir são necessárias:</span><span class="sxs-lookup"><span data-stu-id="ee37a-108">The following subscriptions are required:</span></span>

* <span data-ttu-id="ee37a-109">Azure &mdash; se você não tiver uma conta [Obtenha uma avaliação gratuita](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="ee37a-109">Azure &mdash; If you don't have an account, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="ee37a-110">Os serviços do Azure DevOps &mdash; sua assinatura de DevOps do Azure e a organização é criado no capítulo 4.</span><span class="sxs-lookup"><span data-stu-id="ee37a-110">Azure DevOps Services &mdash; your Azure DevOps subscription and organization is created in Chapter 4.</span></span>
* <span data-ttu-id="ee37a-111">GitHub &mdash; se você não tiver uma conta [Inscreva-se gratuitamente](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="ee37a-111">GitHub &mdash; If you don't have an account, [sign up for free](https://github.com/join).</span></span>

<span data-ttu-id="ee37a-112">As ferramentas a seguir são necessárias:</span><span class="sxs-lookup"><span data-stu-id="ee37a-112">The following tools are required:</span></span>

* <span data-ttu-id="ee37a-113">[Git](https://git-scm.com/downloads) &mdash; um entendimento fundamental do Git é recomendado para este guia.</span><span class="sxs-lookup"><span data-stu-id="ee37a-113">[Git](https://git-scm.com/downloads) &mdash; A fundamental understanding of Git is recommended for this guide.</span></span> <span data-ttu-id="ee37a-114">Examine os [documentação do Git](https://git-scm.com/doc), especificamente [git remoto](https://git-scm.com/docs/git-remote) e [git push](https://git-scm.com/docs/git-push).</span><span class="sxs-lookup"><span data-stu-id="ee37a-114">Review the [Git documentation](https://git-scm.com/doc), specifically [git remote](https://git-scm.com/docs/git-remote) and [git push](https://git-scm.com/docs/git-push).</span></span>
* <span data-ttu-id="ee37a-115">[SDK do .NET core](https://www.microsoft.com/net/download/) &mdash; versão 2.1.300 ou posterior é necessário para compilar e executar o aplicativo de exemplo.</span><span class="sxs-lookup"><span data-stu-id="ee37a-115">[.NET Core SDK](https://www.microsoft.com/net/download/) &mdash; Version 2.1.300 or later is required to build and run the sample app.</span></span> <span data-ttu-id="ee37a-116">Se o Visual Studio é instalado com o **desenvolvimento de plataforma cruzada do .NET Core** carga de trabalho, o SDK do .NET Core já está instalada.</span><span class="sxs-lookup"><span data-stu-id="ee37a-116">If Visual Studio is installed with the **.NET Core cross-platform development** workload, the .NET Core SDK is already installed.</span></span>

    <span data-ttu-id="ee37a-117">Verifique se a instalação do SDK do .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ee37a-117">Verify your .NET Core SDK installation.</span></span> <span data-ttu-id="ee37a-118">Abra um shell de comando e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="ee37a-118">Open a command shell, and run the following command:</span></span>

    ```console
    dotnet --version
    ```

## <a name="recommended-tools-windows-only"></a><span data-ttu-id="ee37a-119">Ferramentas recomendadas (somente Windows)</span><span class="sxs-lookup"><span data-stu-id="ee37a-119">Recommended tools (Windows only)</span></span>

* <span data-ttu-id="ee37a-120">[Visual Studio](https://www.visualstudio.com/)do robustas ferramentas do Azure fornecem uma interface gráfica para a maioria das funcionalidades descritas neste guia.</span><span class="sxs-lookup"><span data-stu-id="ee37a-120">[Visual Studio](https://www.visualstudio.com/)'s robust Azure tools provide a GUI for most of the functionality described in this guide.</span></span> <span data-ttu-id="ee37a-121">Qualquer edição do Visual Studio funcionará, incluindo o Visual Studio Community Edition gratuito.</span><span class="sxs-lookup"><span data-stu-id="ee37a-121">Any edition of Visual Studio will work, including the free Visual Studio Community Edition.</span></span> <span data-ttu-id="ee37a-122">Os tutoriais são gravados para demonstrar o desenvolvimento, implantação e operações de desenvolvimento com e sem o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ee37a-122">The tutorials are written to demonstrate development, deployment, and DevOps both with and without Visual Studio.</span></span>

  <span data-ttu-id="ee37a-123">Confirme se o Visual Studio tem o seguinte [cargas de trabalho](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) instalado:</span><span class="sxs-lookup"><span data-stu-id="ee37a-123">Confirm that Visual Studio has the following [workloads](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) installed:</span></span>

  * <span data-ttu-id="ee37a-124">Desenvolvimento do ASP.NET e para a Web</span><span class="sxs-lookup"><span data-stu-id="ee37a-124">ASP.NET and web development</span></span>
  * <span data-ttu-id="ee37a-125">Desenvolvimento do Azure</span><span class="sxs-lookup"><span data-stu-id="ee37a-125">Azure development</span></span>
  * <span data-ttu-id="ee37a-126">Desenvolvimento de plataforma cruzada do .NET Core</span><span class="sxs-lookup"><span data-stu-id="ee37a-126">.NET Core cross-platform development</span></span>

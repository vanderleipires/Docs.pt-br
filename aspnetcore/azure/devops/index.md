---
title: DevOps com ASP.NET Core e Azure
author: CamSoper
description: Um guia que fornece orientação de ponta a ponta sobre a criação de um pipeline de DevOps para um aplicativo ASP.NET Core hospedado no Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/index
ms.openlocfilehash: 09ca835e908e81c6f38f9430fb40638ba6dc3350
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722523"
---
# <a name="devops-with-aspnet-core-and-azure"></a><span data-ttu-id="2df0b-103">DevOps com ASP.NET Core e Azure</span><span class="sxs-lookup"><span data-stu-id="2df0b-103">DevOps with ASP.NET Core and Azure</span></span>

<span data-ttu-id="2df0b-104">Bem-vindo ao guia de ciclo de vida de desenvolvimento do Azure para .NET.</span><span class="sxs-lookup"><span data-stu-id="2df0b-104">Welcome to the Azure Development Lifecycle guide for .NET!</span></span> <span data-ttu-id="2df0b-105">Este guia apresenta os conceitos básicos da criação de um ciclo de vida de desenvolvimento para o Azure usando processos e ferramentas do .NET.</span><span class="sxs-lookup"><span data-stu-id="2df0b-105">This guide introduces the basic concepts of building a development lifecycle around Azure using .NET tools and processes.</span></span> <span data-ttu-id="2df0b-106">Depois de concluí-lo, você aproveitará os benefícios de uma cadeia de ferramentas madura do DevOps.</span><span class="sxs-lookup"><span data-stu-id="2df0b-106">After finishing this guide, you'll reap the benefits of a mature DevOps toolchain.</span></span>

## <a name="who-this-guide-is-for"></a><span data-ttu-id="2df0b-107">A quem esse guia se destina</span><span class="sxs-lookup"><span data-stu-id="2df0b-107">Who this guide is for</span></span>

<span data-ttu-id="2df0b-108">Você deve ser um desenvolvedor experiente do ASP.NET (nível 200 a 300).</span><span class="sxs-lookup"><span data-stu-id="2df0b-108">You should be an experienced ASP.NET developer (200-300 level).</span></span> <span data-ttu-id="2df0b-109">Você não precisa saber nada sobre o Azure, uma vez que abordaremos isso nesta introdução.</span><span class="sxs-lookup"><span data-stu-id="2df0b-109">You don't need to know anything about Azure, as we'll cover that in this introduction.</span></span> <span data-ttu-id="2df0b-110">Esse guia também pode ser útil para engenheiros de DevOps que estão mais focados nas operações do que no desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="2df0b-110">This guide may also be useful for DevOps engineers who are more focused on operations than development.</span></span>

<span data-ttu-id="2df0b-111">Esse guia destina-se aos desenvolvedores do Windows.</span><span class="sxs-lookup"><span data-stu-id="2df0b-111">This guide targets Windows developers.</span></span> <span data-ttu-id="2df0b-112">No entanto, Linux e macOS são totalmente compatíveis com .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2df0b-112">However, Linux and macOS are fully supported by .NET Core.</span></span> <span data-ttu-id="2df0b-113">Para adaptar esse guia para Linux/macOS, fique atento a textos explicativos para diferenças do Linux/macOS.</span><span class="sxs-lookup"><span data-stu-id="2df0b-113">To adapt this guide for Linux/macOS, watch for callouts for Linux/macOS differences.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="2df0b-114">O que este guia não aborda</span><span class="sxs-lookup"><span data-stu-id="2df0b-114">What this guide doesn't cover</span></span>

<span data-ttu-id="2df0b-115">Esse guia se concentra em uma experiência de implantação contínua de ponta a ponta para desenvolvedores do .NET.</span><span class="sxs-lookup"><span data-stu-id="2df0b-115">This guide is focused on an end-to-end continuous deployment experience for .NET developers.</span></span> <span data-ttu-id="2df0b-116">Ele não é um guia completo para tudo relacionado ao Azure e não se concentra extensivamente em APIs .NET para serviços do Azure.</span><span class="sxs-lookup"><span data-stu-id="2df0b-116">It's not an exhaustive guide to all things Azure, and it doesn't focus extensively on .NET APIs for Azure services.</span></span> <span data-ttu-id="2df0b-117">A ênfase está totalmente concentrada na integração contínua, na implantação, no monitoramento e na depuração.</span><span class="sxs-lookup"><span data-stu-id="2df0b-117">The emphasis is all around continuous integration, deployment, monitoring, and debugging.</span></span> <span data-ttu-id="2df0b-118">No final do guia, são oferecidas recomendações para as próximas etapas.</span><span class="sxs-lookup"><span data-stu-id="2df0b-118">Near the end of the guide, recommendations for next steps are offered.</span></span> <span data-ttu-id="2df0b-119">Serviços da plataforma do Azure que úteis para desenvolvedores do ASP.NET estão incluídos nas sugestões.</span><span class="sxs-lookup"><span data-stu-id="2df0b-119">Included in the suggestions are Azure platform services that are useful to ASP.NET developers.</span></span>

## <a name="whats-in-this-guide"></a><span data-ttu-id="2df0b-120">O que há neste guia</span><span class="sxs-lookup"><span data-stu-id="2df0b-120">What's in this guide</span></span>

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[<span data-ttu-id="2df0b-121">Ferramentas e downloads</span><span class="sxs-lookup"><span data-stu-id="2df0b-121">Tools and downloads</span></span>](xref:azure/devops/tools-and-downloads)

<span data-ttu-id="2df0b-122">Saiba onde obter as ferramentas usadas neste guia.</span><span class="sxs-lookup"><span data-stu-id="2df0b-122">Learn where to acquire the tools used in this guide.</span></span>

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[<span data-ttu-id="2df0b-123">Implantar no Serviço de Aplicativo</span><span class="sxs-lookup"><span data-stu-id="2df0b-123">Deploy to App Service</span></span>](xref:azure/devops/deploy-to-app-service)

<span data-ttu-id="2df0b-124">Aprenda os vários métodos para implantar um aplicativo ASP.NET Core no Serviço de Aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="2df0b-124">Learn the various methods for deploying an ASP.NET Core app to Azure App Service.</span></span>

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[<span data-ttu-id="2df0b-125">Integração contínua e implantação</span><span class="sxs-lookup"><span data-stu-id="2df0b-125">Continuous integration and deployment</span></span>](xref:azure/devops/cicd)

<span data-ttu-id="2df0b-126">Crie uma solução de implantação e integração contínua de ponta a ponta para seu aplicativo ASP.NET Core com o GitHub, o VSTS e o Azure.</span><span class="sxs-lookup"><span data-stu-id="2df0b-126">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, VSTS, and Azure.</span></span>

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[<span data-ttu-id="2df0b-127">Monitorar e depurar</span><span class="sxs-lookup"><span data-stu-id="2df0b-127">Monitor and debug</span></span>](xref:azure/devops/monitor)

<span data-ttu-id="2df0b-128">Use as ferramentas do Azure para monitorar, solucionar problemas e ajustar seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2df0b-128">Use Azure's tools to monitor, troubleshoot, and tune your application.</span></span>

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[<span data-ttu-id="2df0b-129">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="2df0b-129">Next steps</span></span>](xref:azure/devops/next-steps)

<span data-ttu-id="2df0b-130">Outros roteiros de aprendizado para desenvolvedores do ASP.NET Core que estão aprendendo sobre o Azure.</span><span class="sxs-lookup"><span data-stu-id="2df0b-130">Other learning paths for the ASP.NET Core developer learning Azure.</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="2df0b-131">Agradecimentos</span><span class="sxs-lookup"><span data-stu-id="2df0b-131">Acknowledgments</span></span>

<span data-ttu-id="2df0b-132">Obrigado a todos na comunidade do .NET que contribuíram para esse guia com sugestões úteis.</span><span class="sxs-lookup"><span data-stu-id="2df0b-132">Thank you to everyone in the .NET community who contributed to this guide with helpful suggestions!</span></span> <span data-ttu-id="2df0b-133">Gostaríamos de agradecer especificamente os seguintes membros da comunidade que contribuíram para a revisão final deste material:</span><span class="sxs-lookup"><span data-stu-id="2df0b-133">We'd like to specifically thank the following community members who contributed to the final review of this material:</span></span>

* [<span data-ttu-id="2df0b-134">Sam Wronski</span><span class="sxs-lookup"><span data-stu-id="2df0b-134">Sam Wronski</span></span>](https://www.youtube.com/c/worldofzerodevelopment)
* [<span data-ttu-id="2df0b-135">Jeffrey Palermo</span><span class="sxs-lookup"><span data-stu-id="2df0b-135">Jeffrey Palermo</span></span>](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a><span data-ttu-id="2df0b-136">Conclusão</span><span class="sxs-lookup"><span data-stu-id="2df0b-136">Conclusion</span></span>

<span data-ttu-id="2df0b-137">Esse guia prepara você para criar um ciclo de vida de desenvolvimento de integração contínua criado em torno do ASP.NET Core e do Serviço de Aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="2df0b-137">This guide prepares you to build a continuous integration development lifecycle built around ASP.NET Core and Azure App Service.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="2df0b-138">Leitura adicional</span><span class="sxs-lookup"><span data-stu-id="2df0b-138">Additional reading</span></span>

* [<span data-ttu-id="2df0b-139">O que é a computação em nuvem?</span><span class="sxs-lookup"><span data-stu-id="2df0b-139">What is Cloud Computing?</span></span>](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [<span data-ttu-id="2df0b-140">Exemplos de computação em nuvem</span><span class="sxs-lookup"><span data-stu-id="2df0b-140">Examples of Cloud Computing</span></span>](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [<span data-ttu-id="2df0b-141">O que é IaaS?</span><span class="sxs-lookup"><span data-stu-id="2df0b-141">What is IaaS?</span></span>](https://azure.microsoft.com/overview/what-is-iaas/)
* [<span data-ttu-id="2df0b-142">O que é PaaS?</span><span class="sxs-lookup"><span data-stu-id="2df0b-142">What is PaaS?</span></span>](https://azure.microsoft.com/overview/what-is-paas/)

---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Cenário: Configurar um ambiente de teste para a implantação da Web | Microsoft Docs'
author: jrjlee
description: Este tópico descreve um cenário de implantação da web típico para desenvolvedores ou ambientes de teste e explica as tarefas que precisam ser concluídas para configurar um si...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2976be642815e715ac19bd9db34485cf5474cb32
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="scenario-configuring-a-test-environment-for-web-deployment"></a><span data-ttu-id="3bec5-103">Cenário: Configurar um ambiente de teste para a implantação da Web</span><span class="sxs-lookup"><span data-stu-id="3bec5-103">Scenario: Configuring a Test Environment for Web Deployment</span></span>
====================
<span data-ttu-id="3bec5-104">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="3bec5-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="3bec5-105">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="3bec5-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="3bec5-106">Este tópico descreve um cenário de implantação da web típico para desenvolvedores ou ambientes de teste e explica as tarefas que precisam ser concluídas para configurar um ambiente semelhante.</span><span class="sxs-lookup"><span data-stu-id="3bec5-106">This topic describes a typical web deployment scenario for developer or test environments and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="3bec5-107">Quando os desenvolvedores trabalham em aplicativos da web, eles geralmente recebe acesso para um ambiente de servidor que podem ser usadas para testar alterações em seus aplicativos em uma configuração realista.</span><span class="sxs-lookup"><span data-stu-id="3bec5-107">When developers work on web applications, they're often given access to a server environment that they can use to test changes to their applications in a realistic setting.</span></span> <span data-ttu-id="3bec5-108">Normalmente, esse tipo de ambiente de desenvolvimento ou teste tem as seguintes características:</span><span class="sxs-lookup"><span data-stu-id="3bec5-108">This kind of development or test environment typically has these characteristics:</span></span>

- <span data-ttu-id="3bec5-109">O ambiente consiste em um único servidor web e um servidor de banco de dados único.</span><span class="sxs-lookup"><span data-stu-id="3bec5-109">The environment consists of a single web server and a single database server.</span></span>
- <span data-ttu-id="3bec5-110">Os desenvolvedores geralmente têm privilégios de administrador nos servidores que você configurar o ambiente para os requisitos de seus aplicativos.</span><span class="sxs-lookup"><span data-stu-id="3bec5-110">The developers usually have administrator privileges on the servers, to let them configure the environment to the requirements of their applications.</span></span>
- <span data-ttu-id="3bec5-111">Alterações em aplicativos são implantadas com frequência, assim o ambiente precisa dar suporte a apenas uma etapa ou a implantação automatizada.</span><span class="sxs-lookup"><span data-stu-id="3bec5-111">Changes to applications are deployed on a frequent basis, so the environment needs to support single-step or automated deployment.</span></span>

<span data-ttu-id="3bec5-112">Por exemplo, em nosso [cenário do tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink é um desenvolvedor na Fabrikam, Inc. Matt está trabalhando na solução de Gerenciador de contato e regularmente precisa implantar alterações em um ambiente de teste.</span><span class="sxs-lookup"><span data-stu-id="3bec5-112">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt Hink is a developer at Fabrikam, Inc. Matt is working on the Contact Manager solution and regularly needs to deploy changes to a test environment.</span></span> <span data-ttu-id="3bec5-113">Matt é um administrador no servidor web de teste e o servidor de banco de dados de teste.</span><span class="sxs-lookup"><span data-stu-id="3bec5-113">Matt is an administrator on the test web server and the test database server.</span></span> <span data-ttu-id="3bec5-114">Inicialmente, Matt precisa ser capaz de implantar a solução para o ambiente de teste diretamente.</span><span class="sxs-lookup"><span data-stu-id="3bec5-114">Initially, Matt needs to be able to deploy the solution to the test environment directly.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

<span data-ttu-id="3bec5-115">Como trabalho progride e desenvolvedores unir a equipe, o gerente do contato solução está configurada para integração contínua (CI) no Team Foundation Server (TFS).</span><span class="sxs-lookup"><span data-stu-id="3bec5-115">As work progresses and more developers join the team, the Contact Manager solution is configured for continuous integration (CI) in Team Foundation Server (TFS).</span></span> <span data-ttu-id="3bec5-116">Sempre que um desenvolvedor verifica no conteúdo, Team Build deve compilar a solução, executar testes de unidade e implantar a solução automaticamente para o ambiente de teste.</span><span class="sxs-lookup"><span data-stu-id="3bec5-116">Whenever a developer checks in content, Team Build should build the solution, run any unit tests, and automatically deploy the solution to the test environment.</span></span>

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a><span data-ttu-id="3bec5-117">Visão geral da solução</span><span class="sxs-lookup"><span data-stu-id="3bec5-117">Solution Overview</span></span>

<span data-ttu-id="3bec5-118">O ambiente de teste precisa dar suporte a apenas uma etapa ou automatizado implantação de um computador remoto, você tem a opção de duas abordagens principais.</span><span class="sxs-lookup"><span data-stu-id="3bec5-118">The test environment needs to support single-step or automated deployment from a remote computer, so you have a choice of two main approaches.</span></span> <span data-ttu-id="3bec5-119">Você pode:</span><span class="sxs-lookup"><span data-stu-id="3bec5-119">You can:</span></span>

- <span data-ttu-id="3bec5-120">Configure o servidor web de teste para dar suporte à implantação usando o serviço de agente de implantação da Web (o "agente remoto").</span><span class="sxs-lookup"><span data-stu-id="3bec5-120">Configure the test web server to support deployment using the Web Deployment Agent Service (the "remote agent").</span></span>
- <span data-ttu-id="3bec5-121">Configure o servidor web de teste para dar suporte à implantação usando o manipulador de implantação da Web.</span><span class="sxs-lookup"><span data-stu-id="3bec5-121">Configure the test web server to support deployment using the Web Deploy handler.</span></span>

> [!NOTE]
> <span data-ttu-id="3bec5-122">Você também pode usar [Web implantar sob demanda](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (o "agente temp").</span><span class="sxs-lookup"><span data-stu-id="3bec5-122">You could also use [Web Deploy On Demand](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (the "temp agent").</span></span> <span data-ttu-id="3bec5-123">Isso é semelhante à abordagem de agente remoto em termos de requisitos e restrições.</span><span class="sxs-lookup"><span data-stu-id="3bec5-123">This is similar to the remote agent approach in terms of requirements and constraints.</span></span>


<span data-ttu-id="3bec5-124">Nesse caso, os desenvolvedores têm privilégios de administrador no servidor de destino e o ambiente de teste não está sujeito a restrições de segurança estrita, a opção lógica configurar o servidor web de teste para dar suporte à implantação usando o agente remoto.</span><span class="sxs-lookup"><span data-stu-id="3bec5-124">In this case, the developers have administrator privileges on the destination servers, and the test environment is not subject to strict security constraints, so the logical choice is to configure the test web server to support deployment using the remote agent.</span></span> <span data-ttu-id="3bec5-125">Isso é menos complexo e exige a configuração inicial menor que a abordagem de manipulador de implantação da Web.</span><span class="sxs-lookup"><span data-stu-id="3bec5-125">This is less complex and requires less initial configuration than the Web Deploy Handler approach.</span></span> <span data-ttu-id="3bec5-126">Você também precisará configurar o servidor de banco de dados para dar suporte à implantação e acesso remoto.</span><span class="sxs-lookup"><span data-stu-id="3bec5-126">You'll also need to configure your database server to support remote access and deployment.</span></span>

<span data-ttu-id="3bec5-127">Esses tópicos fornecem todas as informações necessárias para concluir essas tarefas:</span><span class="sxs-lookup"><span data-stu-id="3bec5-127">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="3bec5-128">[Configurar um servidor Web para publicação (agente remoto) de implantação da Web](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span><span class="sxs-lookup"><span data-stu-id="3bec5-128">[Configure a Web Server for Web Deploy Publishing (Remote Agent)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).</span></span> <span data-ttu-id="3bec5-129">Este tópico descreve como criar um servidor web que dá suporte à implantação da Web de publicação, usando a abordagem de agente remoto, a partir de uma compilação limpa do Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="3bec5-129">This topic describes how to build a web server that supports Web Deploy publishing, using the remote agent approach, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="3bec5-130">[Configurar um servidor de banco de dados para publicação de implantação da Web](configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="3bec5-130">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="3bec5-131">Este tópico descreve como configurar um servidor de banco de dados para dar suporte à implantação, a partir de uma instalação padrão do SQL Server 2008 R2 e acesso remoto.</span><span class="sxs-lookup"><span data-stu-id="3bec5-131">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="3bec5-132">Leitura adicional</span><span class="sxs-lookup"><span data-stu-id="3bec5-132">Further Reading</span></span>

<span data-ttu-id="3bec5-133">Para obter orientação sobre como configurar um ambiente típico de preparo, consulte [cenário: configurar um ambiente de preparo para a implantação da Web](scenario-configuring-a-staging-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="3bec5-133">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span> <span data-ttu-id="3bec5-134">Para obter orientação sobre como configurar um ambiente de produção típicos, consulte [cenário: configurar um ambiente de produção para implantação de Web](scenario-configuring-a-production-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="3bec5-134">For guidance on configuring a typical production environment, see [Scenario: Configuring a Production Environment for Web Deployment](scenario-configuring-a-production-environment-for-web-deployment.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3bec5-135">[Anterior](choosing-the-right-approach-to-web-deployment.md)
> [Próximo](scenario-configuring-a-staging-environment-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="3bec5-135">[Previous](choosing-the-right-approach-to-web-deployment.md)
[Next](scenario-configuring-a-staging-environment-for-web-deployment.md)</span></span>

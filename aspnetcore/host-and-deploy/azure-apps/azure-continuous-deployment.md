---
title: "Implantação contínua no Azure com o Visual Studio e o Git"
author: rick-anderson
description: "Saiba como criar um aplicativo Web ASP.NET Core usando o Visual Studio e implantá-lo no Serviço de Aplicativo do Azure, usando o Git para implantação contínua."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: c1d25b109bbf211eb476860ff77b649565960b62
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/11/2018
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="c4f83-103">Implantação contínua para o Azure para o ASP.NET Core com o Visual Studio e Git</span><span class="sxs-lookup"><span data-stu-id="c4f83-103">Continuous deployment to Azure for ASP.NET Core with Visual Studio and Git</span></span>

<span data-ttu-id="c4f83-104">Por [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="c4f83-104">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="c4f83-105">Este tutorial mostra como criar um aplicativo web do ASP.NET Core usando o Visual Studio e implantá-lo do Visual Studio para o serviço de aplicativo do Azure usando a implantação contínua.</span><span class="sxs-lookup"><span data-stu-id="c4f83-105">This tutorial shows how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span>

<span data-ttu-id="c4f83-106">Consulte também [Usar o VSTS para criar e publicar um Aplicativo Web do Azure com a implantação contínua](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), que mostra como configurar um fluxo de trabalho de CD (entrega contínua) para o [Serviço de Aplicativo do Azure](/azure/app-service/app-service-web-overview) usando o Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="c4f83-106">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](/azure/app-service/app-service-web-overview) using Visual Studio Team Services.</span></span> <span data-ttu-id="c4f83-107">Entrega contínua do Azure no Team Services simplifica a configuração de um pipeline de implantação robusta para publicar atualizações para aplicativos hospedados no serviço de aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="c4f83-107">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for apps hosted in Azure App Service.</span></span> <span data-ttu-id="c4f83-108">O pipeline pode ser configurado no portal do Azure para criar, executar testes, implantar em um slot de preparo e, em seguida, implantar na produção.</span><span class="sxs-lookup"><span data-stu-id="c4f83-108">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot, and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="c4f83-109">Para concluir este tutorial, é necessária uma conta do Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c4f83-109">To complete this tutorial, a Microsoft Azure account is required.</span></span> <span data-ttu-id="c4f83-110">Para obter uma conta, [ativar os benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) ou [inscrever-se para uma avaliação gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="c4f83-110">To obtain an account, [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) or [sign up for a free trial](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c4f83-111">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="c4f83-111">Prerequisites</span></span>

<span data-ttu-id="c4f83-112">Este tutorial pressupõe que o seguinte software está instalado:</span><span class="sxs-lookup"><span data-stu-id="c4f83-112">This tutorial assumes the following software is installed:</span></span>

* [<span data-ttu-id="c4f83-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4f83-113">Visual Studio</span></span>](https://www.visualstudio.com)
* <span data-ttu-id="c4f83-114">[SDK do .NET core](https://www.microsoft.com/net/download/core) (tempo de execução e as ferramentas)</span><span class="sxs-lookup"><span data-stu-id="c4f83-114">[.NET Core SDK](https://www.microsoft.com/net/download/core) (runtime and tooling)</span></span>
* <span data-ttu-id="c4f83-115">[Git](https://git-scm.com/downloads) para Windows</span><span class="sxs-lookup"><span data-stu-id="c4f83-115">[Git](https://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="c4f83-116">Criar um aplicativo Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c4f83-116">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="c4f83-117">Inicie o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c4f83-117">Start Visual Studio.</span></span>

1. <span data-ttu-id="c4f83-118">No menu **Arquivo**, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-118">From the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="c4f83-119">Selecione o modelo de projeto **Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-119">Select the **ASP.NET Core Web Application** project template.</span></span> <span data-ttu-id="c4f83-120">Ele será exibido em **Instalado** > **Modelos** > **Visual C#** > **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-120">It appears under **Installed** > **Templates** > **Visual C#** > **.NET Core**.</span></span> <span data-ttu-id="c4f83-121">Nomeie o projeto `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="c4f83-121">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="c4f83-122">Selecione a opção **Criar novo repositório Git** e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-122">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![Caixa de diálogo Novo Projeto](azure-continuous-deployment/_static/01-new-project.png)

1. <span data-ttu-id="c4f83-124">Na caixa de diálogo **Novo Projeto ASP.NET Core**, selecione o modelo **Vazio** do ASP.NET Core e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-124">In the **New ASP.NET Core Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Caixa de diálogo Novo Projeto ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> <span data-ttu-id="c4f83-126">A versão mais recente do .NET Core é 2.0.</span><span class="sxs-lookup"><span data-stu-id="c4f83-126">The most recent release of .NET Core is 2.0.</span></span>

### <a name="running-the-web-app-locally"></a><span data-ttu-id="c4f83-127">Executando o aplicativo Web localmente</span><span class="sxs-lookup"><span data-stu-id="c4f83-127">Running the web app locally</span></span>

1. <span data-ttu-id="c4f83-128">Depois que o Visual Studio concluir a criação do aplicativo, execute o aplicativo selecionando **Depurar** > **Iniciar Depuração**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-128">Once Visual Studio finishes creating the app, run the app by selecting **Debug** > **Start Debugging**.</span></span> <span data-ttu-id="c4f83-129">Como alternativa, pressione **F5**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-129">As an alternative, press **F5**.</span></span>

   <span data-ttu-id="c4f83-130">Talvez seja necessário alguns instantes para inicializar o Visual Studio e o novo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c4f83-130">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="c4f83-131">Quando ele for concluído, o navegador mostra o aplicativo em execução.</span><span class="sxs-lookup"><span data-stu-id="c4f83-131">Once it's complete, the browser shows the running app.</span></span>

   ![Janela do navegador mostrando o aplicativo em execução que exibe “Olá, Mundo!”](azure-continuous-deployment/_static/04-browser-runapp.png)

1. <span data-ttu-id="c4f83-133">Depois de revisar o aplicativo Web em execução, feche o navegador e selecione o ícone de "Parar depuração" na barra de ferramentas do Visual Studio para interromper o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c4f83-133">After reviewing the running Web app, close the browser and select the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="c4f83-134">Criar um aplicativo Web no Portal do Azure</span><span class="sxs-lookup"><span data-stu-id="c4f83-134">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="c4f83-135">As seguintes etapas criam um aplicativo web no Portal do Azure:</span><span class="sxs-lookup"><span data-stu-id="c4f83-135">The following steps create a web app in the Azure Portal:</span></span>

1. <span data-ttu-id="c4f83-136">Faça logon na [Portal do Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c4f83-136">Log in to the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="c4f83-137">Selecione **novo** na parte superior esquerda da interface do portal.</span><span class="sxs-lookup"><span data-stu-id="c4f83-137">Select **NEW** at the top left of the portal interface.</span></span>

1. <span data-ttu-id="c4f83-138">Selecione **Web + móvel** > **aplicativo da Web**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-138">Select **Web + Mobile** > **Web App**.</span></span>

   ![Portal do Microsoft Azure: botão Novo: Web + Móvel em Marketplace: botão Aplicativo Web em Aplicativos em Destaque](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. <span data-ttu-id="c4f83-140">Na folha **Aplicativo Web**, insira um valor exclusivo para o **Nome do Serviço de Aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-140">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

   ![Folha Aplicativo Web](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > <span data-ttu-id="c4f83-142">O **nome do serviço de aplicativo** nome deve ser exclusivo.</span><span class="sxs-lookup"><span data-stu-id="c4f83-142">The **App Service Name** name must be unique.</span></span> <span data-ttu-id="c4f83-143">O portal impõe essa regra quando o nome for fornecido.</span><span class="sxs-lookup"><span data-stu-id="c4f83-143">The portal enforces this rule when the name is provided.</span></span> <span data-ttu-id="c4f83-144">Se fornecer um valor diferente, substitua esse valor para cada ocorrência do **SampleWebAppDemo** neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="c4f83-144">If providing a different value, substitute that value for each occurrence of **SampleWebAppDemo** in this tutorial.</span></span>

   <span data-ttu-id="c4f83-145">Também na folha **Aplicativo Web**, selecione um **Plano do Serviço de Aplicativo/Localização** existente ou crie um novo.</span><span class="sxs-lookup"><span data-stu-id="c4f83-145">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="c4f83-146">Se criar um novo plano, selecione a camada de preços, local e outras opções.</span><span class="sxs-lookup"><span data-stu-id="c4f83-146">If creating a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="c4f83-147">Para obter mais informações sobre planos de serviço de aplicativo, consulte [visão geral detalhada de planos de serviço de aplicativo do Azure](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span><span class="sxs-lookup"><span data-stu-id="c4f83-147">For more information on App Service plans, see [Azure App Service plans in-depth overview](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).</span></span>

1. <span data-ttu-id="c4f83-148">Selecione **Criar**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-148">Select **Create**.</span></span> <span data-ttu-id="c4f83-149">Azure provisionará e iniciar o aplicativo da web.</span><span class="sxs-lookup"><span data-stu-id="c4f83-149">Azure will provision and start the web app.</span></span>

   ![Portal do Azure: folha Conceitos Básicos da Demonstração de Aplicativo Web de Exemplo 01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="c4f83-151">Habilitar a publicação do Git no novo aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="c4f83-151">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="c4f83-152">Git é um sistema de controle de versão distribuídos que pode ser usado para implantar um aplicativo da web do serviço de aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="c4f83-152">Git is a distributed version control system that can be used to deploy an Azure App Service web app.</span></span> <span data-ttu-id="c4f83-153">Código de aplicativo da Web é armazenado em um repositório Git local e o código é implantado no Azure por push para um repositório remoto.</span><span class="sxs-lookup"><span data-stu-id="c4f83-153">Web app code is stored in a local Git repository, and the code is deployed to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="c4f83-154">Faça logon na [Portal do Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c4f83-154">Log into the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="c4f83-155">Selecione **serviços de aplicativos** para exibir uma lista dos serviços de aplicativo associado à assinatura do Azure.</span><span class="sxs-lookup"><span data-stu-id="c4f83-155">Select **App Services** to view a list of the app services associated with the Azure subscription.</span></span>

1. <span data-ttu-id="c4f83-156">Selecione o aplicativo web criado na seção anterior deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="c4f83-156">Select the web app created in the previous section of this tutorial.</span></span>

1. <span data-ttu-id="c4f83-157">Na folha **Implantação**, selecione **Opções de implantação** > **Escolher Origem** > **Repositório Git Local**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-157">In the **Deployment** blade, select **Deployment options** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Folha Configurações: folha Origem de implantação: folha Escolher origem](azure-continuous-deployment/_static/deployment-options.png)

1. <span data-ttu-id="c4f83-159">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-159">Select **OK**.</span></span>

1. <span data-ttu-id="c4f83-160">Se as credenciais de implantação para publicar um aplicativo web ou outro aplicativo de serviço de aplicativo ainda não foram configuradas anteriormente, configurá-los agora:</span><span class="sxs-lookup"><span data-stu-id="c4f83-160">If deployment credentials for publishing a web app or other App Service app haven't previously been set up, set them up now:</span></span>

   * <span data-ttu-id="c4f83-161">Selecione **configurações** > **credenciais de implantação**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-161">Select **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="c4f83-162">O **definir credenciais de implantação** folha é exibida.</span><span class="sxs-lookup"><span data-stu-id="c4f83-162">The **Set deployment credentials** blade is displayed.</span></span>
   * <span data-ttu-id="c4f83-163">Crie um nome de usuário e uma senha.</span><span class="sxs-lookup"><span data-stu-id="c4f83-163">Create a user name and password.</span></span> <span data-ttu-id="c4f83-164">Salve a senha para uso posterior ao configurar o Git.</span><span class="sxs-lookup"><span data-stu-id="c4f83-164">Save the password for later use when setting up Git.</span></span>
   * <span data-ttu-id="c4f83-165">Selecione **Salvar**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-165">Select **Save**.</span></span>

1. <span data-ttu-id="c4f83-166">No **aplicativo Web** folha, selecione **configurações** > **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-166">In the **Web App** blade, select **Settings** > **Properties**.</span></span> <span data-ttu-id="c4f83-167">A URL do repositório Git remoto para implantar é mostrada em **URL do GIT**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-167">The URL of the remote Git repository to deploy to is shown under **GIT URL**.</span></span>

1. <span data-ttu-id="c4f83-168">Copie o valor **URL do GIT** para uso posterior no tutorial.</span><span class="sxs-lookup"><span data-stu-id="c4f83-168">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Portal do Azure: folha Propriedades do aplicativo](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="c4f83-170">Publicar o aplicativo web para o serviço de aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="c4f83-170">Publish the web app to Azure App Service</span></span>

<span data-ttu-id="c4f83-171">Nesta seção, crie um repositório Git local usando Visual Studio e envio desse repositório para o Azure para implantar o aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="c4f83-171">In this section, create a local Git repository using Visual Studio and push from that repository to Azure to deploy the web app.</span></span> <span data-ttu-id="c4f83-172">As etapas envolvidas incluem as seguintes:</span><span class="sxs-lookup"><span data-stu-id="c4f83-172">The steps involved include the following:</span></span>

* <span data-ttu-id="c4f83-173">Adicione a configuração do repositório remoto usando o valor da URL de GIT, para que o repositório local pode ser implantado no Azure.</span><span class="sxs-lookup"><span data-stu-id="c4f83-173">Add the remote repository setting using the GIT URL value, so the local repository can be deployed to Azure.</span></span>
* <span data-ttu-id="c4f83-174">Confirme as alterações do projeto.</span><span class="sxs-lookup"><span data-stu-id="c4f83-174">Commit project changes.</span></span>
* <span data-ttu-id="c4f83-175">Enviar por push as alterações do projeto do repositório local para o repositório remoto no Azure.</span><span class="sxs-lookup"><span data-stu-id="c4f83-175">Push project changes from the local repository to the remote repository on Azure.</span></span>

1. <span data-ttu-id="c4f83-176">No **Gerenciador de Soluções**, clique com o botão direito do mouse em **“SampleWebAppDemo” da Solução** e selecione **Confirmar**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-176">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="c4f83-177">O **Team Explorer** é exibido.</span><span class="sxs-lookup"><span data-stu-id="c4f83-177">The **Team Explorer** is displayed.</span></span>

   ![Guia Conectar do Team Explorer](azure-continuous-deployment/_static/10-team-explorer.png)

1. <span data-ttu-id="c4f83-179">No **Team Explorer**, selecione a **Página Inicial** (ícone da página inicial) > **Configurações** > **Configurações do Repositório**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-179">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

1. <span data-ttu-id="c4f83-180">No **programáveis** seção o **configurações do repositório**, selecione **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-180">In the **Remotes** section of the **Repository Settings**, select **Add**.</span></span> <span data-ttu-id="c4f83-181">O **adicionar remoto** caixa de diálogo é exibida.</span><span class="sxs-lookup"><span data-stu-id="c4f83-181">The **Add Remote** dialog box is displayed.</span></span>

1. <span data-ttu-id="c4f83-182">Defina o **Nome** do remoto como **Azure-SampleApp**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-182">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

1. <span data-ttu-id="c4f83-183">Definir o valor de **buscar** para o **URL de Git** que copiados do Azure anteriormente neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="c4f83-183">Set the value for **Fetch** to the **Git URL** that copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="c4f83-184">Observe que essa é a URL que termina com **.git**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-184">Note that this is the URL that ends with **.git**.</span></span>

   ![Caixa de diálogo Editar Remoto](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > <span data-ttu-id="c4f83-186">Como alternativa, especifique o repositório remoto do **janela comando** abrindo o **janela de comando**, alterar para o diretório do projeto e inserir o comando.</span><span class="sxs-lookup"><span data-stu-id="c4f83-186">As an alternative, specify the remote repository from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the command.</span></span> <span data-ttu-id="c4f83-187">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="c4f83-187">Example:</span></span>
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. <span data-ttu-id="c4f83-188">Selecione a **Página Inicial** (ícone da página inicial) > **Configurações** > **Configurações Globais**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-188">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="c4f83-189">Confirme se o nome e endereço de email estão definidas.</span><span class="sxs-lookup"><span data-stu-id="c4f83-189">Confirm that the name and email address are set.</span></span> <span data-ttu-id="c4f83-190">Selecione **atualização** se necessário.</span><span class="sxs-lookup"><span data-stu-id="c4f83-190">Select **Update** if required.</span></span>

1. <span data-ttu-id="c4f83-191">Selecione **Página Inicial** > **Alterações** para retornar à exibição **Alterações**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-191">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

1. <span data-ttu-id="c4f83-192">Insira uma mensagem de confirmação, como **inicial Push #1** e selecione **confirmação**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-192">Enter a commit message, such as **Initial Push #1** and select **Commit**.</span></span> <span data-ttu-id="c4f83-193">Essa ação cria um *confirmação* localmente.</span><span class="sxs-lookup"><span data-stu-id="c4f83-193">This action creates a *commit* locally.</span></span>

   ![Guia Conectar do Team Explorer](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > <span data-ttu-id="c4f83-195">Como alternativa, confirmação muda do **janela comando** abrindo o **janela de comando**, alterar para o diretório do projeto e digitar os comandos do git.</span><span class="sxs-lookup"><span data-stu-id="c4f83-195">As an alternative, commit changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering the git commands.</span></span> <span data-ttu-id="c4f83-196">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="c4f83-196">Example:</span></span>
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. <span data-ttu-id="c4f83-197">Selecione **Página Inicial** > **Sincronização** > **Ações** > **Abrir Prompt de Comando**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-197">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="c4f83-198">Abre o prompt de comando para o diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="c4f83-198">The command prompt opens to the project directory.</span></span>

1. <span data-ttu-id="c4f83-199">Insira o seguinte comando na janela de comando:</span><span class="sxs-lookup"><span data-stu-id="c4f83-199">Enter the following command in the command window:</span></span>

   `git push -u Azure-SampleApp master`

1. <span data-ttu-id="c4f83-200">Insira o Azure **credenciais de implantação** senha criados anteriormente no Azure.</span><span class="sxs-lookup"><span data-stu-id="c4f83-200">Enter the Azure **deployment credentials** password created earlier in Azure.</span></span>

   <span data-ttu-id="c4f83-201">Esse comando inicia o processo de envio por push os arquivos do projeto local para o Azure.</span><span class="sxs-lookup"><span data-stu-id="c4f83-201">This command starts the process of pushing the local project files to Azure.</span></span> <span data-ttu-id="c4f83-202">A saída do comando acima termina com uma mensagem de que a implantação foi bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="c4f83-202">The output from the above command ends with a message that the deployment was successful.</span></span>

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > <span data-ttu-id="c4f83-203">Se a colaboração no projeto é necessária, considere a possibilidade de envio por push para [GitHub](https://github.com) antes de enviar por push para o Azure.</span><span class="sxs-lookup"><span data-stu-id="c4f83-203">If collaboration on the project is required, consider pushing to [GitHub](https://github.com) before pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="c4f83-204">Verificar a implantação ativa</span><span class="sxs-lookup"><span data-stu-id="c4f83-204">Verify the Active Deployment</span></span>

<span data-ttu-id="c4f83-205">Verifique se a transferência de aplicativos web do ambiente local para o Azure é bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="c4f83-205">Verify that the web app transfer from the local environment to Azure is successful.</span></span>

<span data-ttu-id="c4f83-206">No [Portal do Azure](https://portal.azure.com), selecione o aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="c4f83-206">In the [Azure Portal](https://portal.azure.com), select the web app.</span></span> <span data-ttu-id="c4f83-207">Selecione **implantação** > **opções de implantação**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-207">Select **Deployment** > **Deployment options**.</span></span>

![Portal do Azure: folha Configurações: folha Implantações mostrando a implantação bem-sucedida](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="c4f83-209">Executar o aplicativo no Azure</span><span class="sxs-lookup"><span data-stu-id="c4f83-209">Run the app in Azure</span></span>

<span data-ttu-id="c4f83-210">Agora que o aplicativo web é implantado no Azure, execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c4f83-210">Now that the web app is deployed to Azure, run the app.</span></span>

<span data-ttu-id="c4f83-211">Isso pode ser feito de duas maneiras:</span><span class="sxs-lookup"><span data-stu-id="c4f83-211">This can be accomplished in two ways:</span></span>

* <span data-ttu-id="c4f83-212">No Portal do Azure, localize a folha do aplicativo web para o aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="c4f83-212">In the Azure Portal, locate the web app blade for the web app.</span></span> <span data-ttu-id="c4f83-213">Selecione **procurar** para exibir o aplicativo no navegador padrão.</span><span class="sxs-lookup"><span data-stu-id="c4f83-213">Select **Browse** to view the app in the default browser.</span></span>
* <span data-ttu-id="c4f83-214">Abra um navegador e digite a URL do aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="c4f83-214">Open a browser and enter the URL for the web app.</span></span> <span data-ttu-id="c4f83-215">Exemplo: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="c4f83-215">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="update-the-web-app-and-republish"></a><span data-ttu-id="c4f83-216">Atualizar o aplicativo web e publicar novamente</span><span class="sxs-lookup"><span data-stu-id="c4f83-216">Update the web app and republish</span></span>

<span data-ttu-id="c4f83-217">Depois de fazer alterações no código local, republicar:</span><span class="sxs-lookup"><span data-stu-id="c4f83-217">After making changes to the local code, republish:</span></span>

1. <span data-ttu-id="c4f83-218">No **Gerenciador de Soluções** do Visual Studio, abra o arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c4f83-218">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

1. <span data-ttu-id="c4f83-219">No método `Configure`, modifique o método `Response.WriteAsync` para que ele seja exibido da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c4f83-219">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. <span data-ttu-id="c4f83-220">Salvar as alterações em *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c4f83-220">Save the changes to *Startup.cs*.</span></span>

1. <span data-ttu-id="c4f83-221">No **Gerenciador de Soluções**, clique com o botão direito do mouse em **“SampleWebAppDemo” da Solução** e selecione **Confirmar**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-221">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="c4f83-222">O **Team Explorer** é exibido.</span><span class="sxs-lookup"><span data-stu-id="c4f83-222">The **Team Explorer** is displayed.</span></span>

1. <span data-ttu-id="c4f83-223">Insira uma mensagem de confirmação, como `Update #2`.</span><span class="sxs-lookup"><span data-stu-id="c4f83-223">Enter a commit message, such as `Update #2`.</span></span>

1. <span data-ttu-id="c4f83-224">Pressione o botão **Confirmar** para confirmar as alterações do projeto.</span><span class="sxs-lookup"><span data-stu-id="c4f83-224">Press the **Commit** button to commit the project changes.</span></span>

1. <span data-ttu-id="c4f83-225">Selecione **Página Inicial** > **Sincronização** > **Ações** > **Enviar por Push**.</span><span class="sxs-lookup"><span data-stu-id="c4f83-225">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

> [!NOTE]
> <span data-ttu-id="c4f83-226">Como alternativa, enviar por push as alterações do **janela comando** abrindo o **janela de comando**, alterar para o diretório do projeto e inserir um comando do git.</span><span class="sxs-lookup"><span data-stu-id="c4f83-226">As an alternative, push the changes from the **Command Window** by opening the **Command Window**, changing to the project directory, and entering a git command.</span></span> <span data-ttu-id="c4f83-227">Exemplo:</span><span class="sxs-lookup"><span data-stu-id="c4f83-227">Example:</span></span>
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="c4f83-228">Exibir o aplicativo Web atualizado no Azure</span><span class="sxs-lookup"><span data-stu-id="c4f83-228">View the updated web app in Azure</span></span>

<span data-ttu-id="c4f83-229">Exibir o aplicativo web atualizadas selecionando **procurar** na folha de aplicativo web no Portal do Azure ou abrindo um navegador e digitando a URL do aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="c4f83-229">View the updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for the web app.</span></span> <span data-ttu-id="c4f83-230">Exemplo: `http://SampleWebAppDemo.azurewebsites.net`</span><span class="sxs-lookup"><span data-stu-id="c4f83-230">Example: `http://SampleWebAppDemo.azurewebsites.net`</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c4f83-231">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="c4f83-231">Additional resources</span></span>

* [<span data-ttu-id="c4f83-232">Usar o VSTS para criar e publicar um aplicativo Web do Azure com implantação contínua</span><span class="sxs-lookup"><span data-stu-id="c4f83-232">Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment</span></span>](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [<span data-ttu-id="c4f83-233">Kudu do projeto</span><span class="sxs-lookup"><span data-stu-id="c4f83-233">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)

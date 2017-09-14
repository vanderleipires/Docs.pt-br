---
title: "Implantação contínua no Azure com o Visual Studio e o Git"
author: rick-anderson
description: "Saiba como criar um aplicativo Web ASP.NET Core usando o Visual Studio e implantá-lo no Serviço de Aplicativo do Azure, usando o Git para implantação contínua."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: b576ef6bce3b211afe7465f33dfe62c25dac1f62
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a><span data-ttu-id="518c4-104">Implantação contínua no Azure para o ASP.NET Core, com o Visual Studio e o Git</span><span class="sxs-lookup"><span data-stu-id="518c4-104">Continuous deployment to Azure for ASP.NET Core, with Visual Studio and Git</span></span>

<span data-ttu-id="518c4-105">Por [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="518c4-105">By [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="518c4-106">Este tutorial mostra como criar um aplicativo Web ASP.NET Core usando o Visual Studio e implantá-lo por meio do Visual Studio no Serviço de Aplicativo do Azure usando a implantação contínua.</span><span class="sxs-lookup"><span data-stu-id="518c4-106">This tutorial shows you how to create an ASP.NET Core web app using Visual Studio and deploy it from Visual Studio to Azure App Service using continuous deployment.</span></span> 

<span data-ttu-id="518c4-107">Consulte também [Usar o VSTS para criar e publicar um Aplicativo Web do Azure com a implantação contínua](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), que mostra como configurar um fluxo de trabalho de CD (entrega contínua) para o [Serviço de Aplicativo do Azure](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) usando o Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="518c4-107">See also [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic), which shows how to configure a continuous delivery (CD) workflow for [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) using Visual Studio Team Services.</span></span> <span data-ttu-id="518c4-108">A Entrega Contínua do Azure no Team Services simplifica a configuração de um pipeline de implantação robusta para publicar atualizações do aplicativo no Serviço de Aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="518c4-108">Azure Continuous Delivery in Team Services simplifies setting up a robust deployment pipeline to publish updates for your app to Azure App Service.</span></span> <span data-ttu-id="518c4-109">O pipeline pode ser configurado no portal do Azure para criar, executar testes, implantar em um slot de preparo e, em seguida, implantar na produção.</span><span class="sxs-lookup"><span data-stu-id="518c4-109">The pipeline can be configured from the Azure portal to build, run tests, deploy to a staging slot,  and then deploy to production.</span></span>

> [!NOTE]
> <span data-ttu-id="518c4-110">Para concluir este tutorial, você precisa de uma conta do Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="518c4-110">To complete this tutorial, you need a Microsoft Azure account.</span></span> <span data-ttu-id="518c4-111">Caso não tenha uma conta, [ative seus benefícios do assinante do MSDN](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) ou [inscreva-se em uma avaliação gratuita](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="518c4-111">If you don't have an account, you can [activate your MSDN subscriber benefits](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) or [sign up for a free trial](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="518c4-112">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="518c4-112">Prerequisites</span></span>

<span data-ttu-id="518c4-113">Este tutorial pressupõe que você já instalou o seguinte:</span><span class="sxs-lookup"><span data-stu-id="518c4-113">This tutorial assumes you have already installed the following:</span></span>

* [<span data-ttu-id="518c4-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="518c4-114">Visual Studio</span></span>](https://www.visualstudio.com)

* <span data-ttu-id="518c4-115">[ASP.NET Core](http://go.microsoft.com/fwlink/?LinkId=627627) (tempo de execução e ferramentas)</span><span class="sxs-lookup"><span data-stu-id="518c4-115">[ASP.NET Core](http://go.microsoft.com/fwlink/?LinkId=627627) (runtime and tooling)</span></span>

* <span data-ttu-id="518c4-116">[Git](http://git-scm.com/downloads) para Windows</span><span class="sxs-lookup"><span data-stu-id="518c4-116">[Git](http://git-scm.com/downloads) for Windows</span></span>

## <a name="create-an-aspnet-core-web-app"></a><span data-ttu-id="518c4-117">Criar um aplicativo Web ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="518c4-117">Create an ASP.NET Core web app</span></span>

1. <span data-ttu-id="518c4-118">Inicie o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="518c4-118">Start Visual Studio.</span></span>

2. <span data-ttu-id="518c4-119">No menu **Arquivo**, selecione **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="518c4-119">From the **File** menu, select **New** > **Project**.</span></span>

3. <span data-ttu-id="518c4-120">Selecione o modelo de projeto **Aplicativo Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="518c4-120">Select the **ASP.NET Web Application** project template.</span></span> <span data-ttu-id="518c4-121">Ele será exibido em **Instalado** > **Modelos** > **Visual C#** > **Web**.</span><span class="sxs-lookup"><span data-stu-id="518c4-121">It appears under **Installed** > **Templates** > **Visual C#** > **Web**.</span></span> <span data-ttu-id="518c4-122">Nomeie o projeto `SampleWebAppDemo`.</span><span class="sxs-lookup"><span data-stu-id="518c4-122">Name the project `SampleWebAppDemo`.</span></span> <span data-ttu-id="518c4-123">Selecione a opção **Criar novo repositório Git** e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="518c4-123">Select the **Create new Git respository** option and click **OK**.</span></span>

   ![Caixa de diálogo Novo Projeto](azure-continuous-deployment/_static/01-new-project.png)

4. <span data-ttu-id="518c4-125">Na caixa de diálogo **Novo Projeto ASP.NET**, selecione o modelo **Vazio** do ASP.NET Core e, em seguida, clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="518c4-125">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template, then click **OK**.</span></span>

   ![Caixa de diálogo Novo Projeto ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)


### <a name="running-the-web-app-locally"></a><span data-ttu-id="518c4-127">Executando o aplicativo Web localmente</span><span class="sxs-lookup"><span data-stu-id="518c4-127">Running the web app locally</span></span>

1. <span data-ttu-id="518c4-128">Depois que o Visual Studio concluir a criação do aplicativo, execute o aplicativo selecionando **Depurar** -> **Iniciar Depuração**.</span><span class="sxs-lookup"><span data-stu-id="518c4-128">Once Visual Studio finishes creating the app, run the app by selecting **Debug** -> **Start Debugging**.</span></span> <span data-ttu-id="518c4-129">Como alternativa, você pode pressionar **F5**.</span><span class="sxs-lookup"><span data-stu-id="518c4-129">As an alternative, you can press **F5**.</span></span>

   <span data-ttu-id="518c4-130">Talvez seja necessário alguns instantes para inicializar o Visual Studio e o novo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="518c4-130">It may take time to initialize Visual Studio and the new app.</span></span> <span data-ttu-id="518c4-131">Quando isso for concluído, o navegador mostrará o aplicativo em execução.</span><span class="sxs-lookup"><span data-stu-id="518c4-131">Once it is complete, the browser will show the running app.</span></span>

   ![Janela do navegador mostrando o aplicativo em execução que exibe “Olá, Mundo!”](azure-continuous-deployment/_static/04-browser-runapp.png)

2. <span data-ttu-id="518c4-133">Depois de examinar o aplicativo Web em execução, feche o navegador e clique no ícone “Parar depuração” na barra de ferramentas do Visual Studio para interromper o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="518c4-133">After reviewing the running Web app, close the browser and click the "Stop Debugging" icon in the toolbar of Visual Studio to stop the app.</span></span>

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="518c4-134">Criar um aplicativo Web no Portal do Azure</span><span class="sxs-lookup"><span data-stu-id="518c4-134">Create a web app in the Azure Portal</span></span>

<span data-ttu-id="518c4-135">As etapas a seguir orientarão você pela criação de um aplicativo Web no Portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="518c4-135">The following steps will guide you through creating a web app in the Azure Portal.</span></span>

1. <span data-ttu-id="518c4-136">Entre no [Portal do Azure](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="518c4-136">Log in to the [Azure Portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="518c4-137">Toque em **NOVO** na parte superior esquerda do Portal</span><span class="sxs-lookup"><span data-stu-id="518c4-137">TAP **NEW** at the top left of the Portal</span></span>
3. <span data-ttu-id="518c4-138">Toque em **Web + Móvel** > **Aplicativo Web**</span><span class="sxs-lookup"><span data-stu-id="518c4-138">TAP **Web + Mobile** > **Web App**</span></span>

    ![Portal do Microsoft Azure: botão Novo: Web + Móvel em Marketplace: botão Aplicativo Web em Aplicativos em Destaque](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  <span data-ttu-id="518c4-140">Na folha **Aplicativo Web**, insira um valor exclusivo para o **Nome do Serviço de Aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="518c4-140">In the **Web App** blade, enter a unique value for the **App Service Name**.</span></span>

    ![Folha Aplicativo Web](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    ><span data-ttu-id="518c4-142">O **Nome do Serviço de Aplicativo** precisa ser exclusivo.</span><span class="sxs-lookup"><span data-stu-id="518c4-142">The **App Service Name** name needs to be unique.</span></span> <span data-ttu-id="518c4-143">O portal imporá essa regra quando você tentar inserir o nome.</span><span class="sxs-lookup"><span data-stu-id="518c4-143">The portal will enforce this rule when you attempt to enter the name.</span></span> <span data-ttu-id="518c4-144">Depois de inserir outro valor, você precisará substituir esse valor por cada ocorrência do **SampleWebAppDemo** vista neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="518c4-144">After you enter a different value, you'll need to substitute that value for each occurrence of **SampleWebAppDemo** that you see in this tutorial.</span></span>

    &nbsp;
    
    <span data-ttu-id="518c4-145">Também na folha **Aplicativo Web**, selecione um **Plano do Serviço de Aplicativo/Localização** existente ou crie um novo.</span><span class="sxs-lookup"><span data-stu-id="518c4-145">Also in the **Web App** blade, select an existing **App Service Plan/Location** or create a new one.</span></span> <span data-ttu-id="518c4-146">Se você criar um novo plano, selecione o tipo de preço, o local e outras opções.</span><span class="sxs-lookup"><span data-stu-id="518c4-146">If you create a new plan, select the pricing tier, location, and other options.</span></span> <span data-ttu-id="518c4-147">Para obter mais informações sobre os planos do Serviço de Aplicativo, consulte [Visão geral detalhada dos planos do Serviço de Aplicativo do Azure](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span><span class="sxs-lookup"><span data-stu-id="518c4-147">For more information on App Service plans, [Azure App Service plans in-depth overview](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).</span></span>

5.  <span data-ttu-id="518c4-148">Clique em **Criar**.</span><span class="sxs-lookup"><span data-stu-id="518c4-148">Click **Create**.</span></span> <span data-ttu-id="518c4-149">O Azure provisionará e iniciará o aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="518c4-149">Azure will provision and start your web app.</span></span>

    ![Portal do Azure: folha Conceitos Básicos da Demonstração de Aplicativo Web de Exemplo 01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a><span data-ttu-id="518c4-151">Habilitar a publicação do Git no novo aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="518c4-151">Enable Git publishing for the new web app</span></span>

<span data-ttu-id="518c4-152">O Git é um sistema de controle de versão distribuída que pode ser usado para implantar o aplicativo Web do Serviço de Aplicativo do Azure.</span><span class="sxs-lookup"><span data-stu-id="518c4-152">Git is a distributed version control system that you can use to deploy your Azure App Service web app.</span></span> <span data-ttu-id="518c4-153">Você armazenará o código escrito no aplicativo Web em um repositório Git local e implantará o código no Azure enviando-o por push para um repositório remoto.</span><span class="sxs-lookup"><span data-stu-id="518c4-153">You'll store the code you write for your web app in a local Git repository, and you'll deploy your code to Azure by pushing to a remote repository.</span></span>

1. <span data-ttu-id="518c4-154">Faça logon no [Portal do Azure](https://portal.azure.com), caso ainda não esteja conectado.</span><span class="sxs-lookup"><span data-stu-id="518c4-154">Log into the [Azure Portal](https://portal.azure.com), if you're not already logged in.</span></span>

2. <span data-ttu-id="518c4-155">Clique em **Procurar**, localizado na parte inferior do painel de navegação.</span><span class="sxs-lookup"><span data-stu-id="518c4-155">Click **Browse**, located at the bottom of the navigation pane.</span></span>

3. <span data-ttu-id="518c4-156">Clique em **Aplicativos Web** para exibir uma lista dos aplicativos Web associados à sua assinatura do Azure.</span><span class="sxs-lookup"><span data-stu-id="518c4-156">Click **Web Apps** to view a list of the web apps associated with your Azure subscription.</span></span>

4. <span data-ttu-id="518c4-157">Selecione o aplicativo Web criado na seção anterior deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="518c4-157">Select the web app you created in the previous section of this tutorial.</span></span>

5. <span data-ttu-id="518c4-158">Se a folha **Configurações** não for mostrada, selecione **Configurações** na folha **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="518c4-158">If the **Settings** blade is not shown, select **Settings** in the **Web App** blade.</span></span>

6. <span data-ttu-id="518c4-159">Na folha **Configurações**, selecione **Origem de implantação** > **Escolher Origem** > **Repositório Git Local**.</span><span class="sxs-lookup"><span data-stu-id="518c4-159">In the **Settings** blade, select **Deployment source** > **Choose Source** > **Local Git Repository**.</span></span>

   ![Folha Configurações: folha Origem de implantação: folha Escolher origem](azure-continuous-deployment/_static/08-azure-localrepository.png)

7. <span data-ttu-id="518c4-161">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="518c4-161">Click **OK**.</span></span>

8. <span data-ttu-id="518c4-162">Caso você não tenha configurado anteriormente as credenciais de implantação para publicar um aplicativo Web ou outro aplicativo do Serviço de Aplicativo, configure-as agora:</span><span class="sxs-lookup"><span data-stu-id="518c4-162">If you have not previously set up deployment credentials for publishing a web app or other App Service app, set them up now:</span></span>

   * <span data-ttu-id="518c4-163">Clique em **Configurações** > **Credenciais de implantação**.</span><span class="sxs-lookup"><span data-stu-id="518c4-163">Click **Settings** > **Deployment credentials**.</span></span> <span data-ttu-id="518c4-164">A folha **Definir credenciais de implantação** será exibida.</span><span class="sxs-lookup"><span data-stu-id="518c4-164">The **Set deployment credentials** blade will be displayed.</span></span>

   * <span data-ttu-id="518c4-165">Crie um nome de usuário e uma senha.</span><span class="sxs-lookup"><span data-stu-id="518c4-165">Create a user name and password.</span></span>  <span data-ttu-id="518c4-166">Você precisará dessa senha mais tarde ao configurar o Git.</span><span class="sxs-lookup"><span data-stu-id="518c4-166">You'll need this password later when setting up Git.</span></span>

   * <span data-ttu-id="518c4-167">Clique em **Salvar**.</span><span class="sxs-lookup"><span data-stu-id="518c4-167">Click **Save**.</span></span>

9. <span data-ttu-id="518c4-168">Na folha **Aplicativo Web**, clique em **Configurações** > **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="518c4-168">In the **Web App** blade, click **Settings** > **Properties**.</span></span> <span data-ttu-id="518c4-169">A URL do repositório Git remoto no qual você implantará é mostrada em **URL do GIT**.</span><span class="sxs-lookup"><span data-stu-id="518c4-169">The URL of the remote Git repository that you'll deploy to is shown under **GIT URL**.</span></span>

10. <span data-ttu-id="518c4-170">Copie o valor **URL do GIT** para uso posterior no tutorial.</span><span class="sxs-lookup"><span data-stu-id="518c4-170">Copy the **GIT URL** value for later use in the tutorial.</span></span>

   ![Portal do Azure: folha Propriedades do aplicativo](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a><span data-ttu-id="518c4-172">Publicar o aplicativo Web no Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="518c4-172">Publish your web app to Azure App Service</span></span>

<span data-ttu-id="518c4-173">Nesta seção, você criará um repositório Git local usando o Visual Studio e enviará por push desse repositório para o Azure para implantar o aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="518c4-173">In this section, you will create a local Git repository using Visual Studio and push from that repository to Azure to deploy your web app.</span></span> <span data-ttu-id="518c4-174">As etapas envolvidas incluem as seguintes:</span><span class="sxs-lookup"><span data-stu-id="518c4-174">The steps involved include the following:</span></span>

   * <span data-ttu-id="518c4-175">Adicione a configuração do repositório remoto usando o valor de URL do GIT, de modo que você possa implantar seu repositório local no Azure.</span><span class="sxs-lookup"><span data-stu-id="518c4-175">Add the remote repository setting using your GIT URL value, so you can deploy your local repository to Azure.</span></span>

   * <span data-ttu-id="518c4-176">Confirme as alterações do projeto.</span><span class="sxs-lookup"><span data-stu-id="518c4-176">Commit your project changes.</span></span>

   * <span data-ttu-id="518c4-177">Envie as alterações do projeto por push do repositório local para o repositório remoto no Azure.</span><span class="sxs-lookup"><span data-stu-id="518c4-177">Push your project changes from your local repository to your remote repository on Azure.</span></span>

&nbsp;
   
1.  <span data-ttu-id="518c4-178">No **Gerenciador de Soluções**, clique com o botão direito do mouse em **“SampleWebAppDemo” da Solução** e selecione **Confirmar**.</span><span class="sxs-lookup"><span data-stu-id="518c4-178">In **Solution Explorer** right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="518c4-179">O **Team Explorer** será exibido.</span><span class="sxs-lookup"><span data-stu-id="518c4-179">The **Team Explorer** will be displayed.</span></span>

    ![Guia Conectar do Team Explorer](azure-continuous-deployment/_static/10-team-explorer.png)

2.  <span data-ttu-id="518c4-181">No **Team Explorer**, selecione a **Página Inicial** (ícone da página inicial) > **Configurações** > **Configurações do Repositório**.</span><span class="sxs-lookup"><span data-stu-id="518c4-181">In **Team Explorer**, select the **Home** (home icon) > **Settings** > **Repository Settings**.</span></span>

3.  <span data-ttu-id="518c4-182">Na seção **Remotos** das **Configurações do Repositório**, selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="518c4-182">In the **Remotes** section of the **Repository Settings** select **Add**.</span></span> <span data-ttu-id="518c4-183">A caixa de diálogo **Adicionar Remoto** será exibida.</span><span class="sxs-lookup"><span data-stu-id="518c4-183">The **Add Remote** dialog box will be displayed.</span></span>

4.  <span data-ttu-id="518c4-184">Defina o **Nome** do remoto como **Azure-SampleApp**.</span><span class="sxs-lookup"><span data-stu-id="518c4-184">Set the **Name** of the remote to **Azure-SampleApp**.</span></span>

5.  <span data-ttu-id="518c4-185">Defina o valor de **Buscar** como a **URL do Git** copiada do Azure anteriormente neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="518c4-185">Set the value for **Fetch** to the **Git URL** that you copied from Azure earlier in this tutorial.</span></span> <span data-ttu-id="518c4-186">Observe que essa é a URL que termina com **.git**.</span><span class="sxs-lookup"><span data-stu-id="518c4-186">Note that this is the URL that ends with **.git**.</span></span>

    ![Caixa de diálogo Editar Remoto](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    ><span data-ttu-id="518c4-188">Como alternativa, você pode especificar o repositório remoto na **Janela Comando** abrindo a **Janela Comando**, alterando para o diretório do projeto e inserindo o comando.</span><span class="sxs-lookup"><span data-stu-id="518c4-188">As an alternative, you can specify the remote repository from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the command.</span></span> <span data-ttu-id="518c4-189">Por exemplo: `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span><span class="sxs-lookup"><span data-stu-id="518c4-189">For example:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`</span></span>

6.  <span data-ttu-id="518c4-190">Selecione a **Página Inicial** (ícone da página inicial) > **Configurações** > **Configurações Globais**.</span><span class="sxs-lookup"><span data-stu-id="518c4-190">Select the **Home** (home icon) > **Settings** > **Global Settings**.</span></span> <span data-ttu-id="518c4-191">Verifique se você tem o nome e o endereço de email definidos.</span><span class="sxs-lookup"><span data-stu-id="518c4-191">Make sure you have your name and your email address set.</span></span> <span data-ttu-id="518c4-192">Talvez você também precise selecionar **Atualizar**.</span><span class="sxs-lookup"><span data-stu-id="518c4-192">You may also need to select **Update**.</span></span>

7.  <span data-ttu-id="518c4-193">Selecione **Página Inicial** > **Alterações** para retornar à exibição **Alterações**.</span><span class="sxs-lookup"><span data-stu-id="518c4-193">Select **Home** > **Changes** to return to the **Changes** view.</span></span>

8.  <span data-ttu-id="518c4-194">Escreva uma mensagem de confirmação, como **Push Inicial nº 1** e clique em **Confirmar**.</span><span class="sxs-lookup"><span data-stu-id="518c4-194">Enter a commit message, such as **Initial Push #1** and click **Commit**.</span></span> <span data-ttu-id="518c4-195">Essa ação criará uma *confirmação* localmente.</span><span class="sxs-lookup"><span data-stu-id="518c4-195">This action will create a *commit* locally.</span></span> <span data-ttu-id="518c4-196">Em seguida, você precisa *sincronizar* com o Azure.</span><span class="sxs-lookup"><span data-stu-id="518c4-196">Next, you need to *sync* with Azure.</span></span>

    ![Guia Conectar do Team Explorer](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    ><span data-ttu-id="518c4-198">Como alternativa, você pode confirmar as alterações na **Janela Comando** abrindo a **Janela Comando**, alterando para o diretório do projeto e inserindo os comandos do Git.</span><span class="sxs-lookup"><span data-stu-id="518c4-198">As an alternative, you can commit your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering the git commands.</span></span> <span data-ttu-id="518c4-199">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="518c4-199">For example:</span></span>
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  <span data-ttu-id="518c4-200">Selecione **Página Inicial** > **Sincronização** > **Ações** > **Abrir Prompt de Comando**.</span><span class="sxs-lookup"><span data-stu-id="518c4-200">Select **Home** > **Sync** > **Actions** > **Open Command Prompt**.</span></span> <span data-ttu-id="518c4-201">O prompt de comando será aberto no diretório do projeto.</span><span class="sxs-lookup"><span data-stu-id="518c4-201">The command prompt will open to your project directory.</span></span>

10.  <span data-ttu-id="518c4-202">Insira o seguinte comando na janela de comando:</span><span class="sxs-lookup"><span data-stu-id="518c4-202">Enter the following command in the command window:</span></span>

    `git push -u Azure-SampleApp master`

11.  <span data-ttu-id="518c4-203">Insira a senha das **credenciais de implantação** do Azure criada anteriormente no Azure.</span><span class="sxs-lookup"><span data-stu-id="518c4-203">Enter your Azure **deployment credentials** password that you created earlier in Azure.</span></span>

    >[!NOTE]
    ><span data-ttu-id="518c4-204">Sua senha não será visível enquanto você a digita.</span><span class="sxs-lookup"><span data-stu-id="518c4-204">Your password will not be visible as you enter it.</span></span>

    <span data-ttu-id="518c4-205">Esse comando iniciará o processo de envio por push dos arquivos de projeto locais para o Azure.</span><span class="sxs-lookup"><span data-stu-id="518c4-205">This command will start the process of pushing your local project files to Azure.</span></span> <span data-ttu-id="518c4-206">O resultado do comando acima termina com uma mensagem informando que a implantação foi bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="518c4-206">The output from the above command ends with a message that deployment was successful.</span></span>
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > <span data-ttu-id="518c4-207">Se precisar colaborar em um projeto, considere a possibilidade de envio por push para o [GitHub](https://github.com) entre o envio por push para o Azure.</span><span class="sxs-lookup"><span data-stu-id="518c4-207">If you need to collaborate on a project, you should consider pushing to [GitHub](https://github.com) in between pushing to Azure.</span></span>
 
### <a name="verify-the-active-deployment"></a><span data-ttu-id="518c4-208">Verificar a implantação ativa</span><span class="sxs-lookup"><span data-stu-id="518c4-208">Verify the Active Deployment</span></span>

<span data-ttu-id="518c4-209">Verifique se você transferiu com êxito o aplicativo Web do ambiente local para o Azure.</span><span class="sxs-lookup"><span data-stu-id="518c4-209">You can verify that you successfully transferred the web app from your local environment to Azure.</span></span> <span data-ttu-id="518c4-210">Você verá a implantação bem-sucedida listada.</span><span class="sxs-lookup"><span data-stu-id="518c4-210">You'll see the listed successful deployment.</span></span>

1. <span data-ttu-id="518c4-211">No [Portal do Azure](https://portal.azure.com), selecione o aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="518c4-211">In the [Azure Portal](https://portal.azure.com), select your web app.</span></span> <span data-ttu-id="518c4-212">Em seguida, selecione **Configurações** > **Implantação contínua**.</span><span class="sxs-lookup"><span data-stu-id="518c4-212">Then, select **Settings** > **Continuous deployment**.</span></span>

   ![Portal do Azure: folha Configurações: folha Implantações mostrando a implantação bem-sucedida](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a><span data-ttu-id="518c4-214">Executar o aplicativo no Azure</span><span class="sxs-lookup"><span data-stu-id="518c4-214">Run the app in Azure</span></span>

<span data-ttu-id="518c4-215">Agora que você implantou o aplicativo Web para o Azure, poderá executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="518c4-215">Now that you have deployed your web app to Azure, you can run the app.</span></span>

<span data-ttu-id="518c4-216">Isso pode ser feito de duas maneiras:</span><span class="sxs-lookup"><span data-stu-id="518c4-216">This can be done in two ways:</span></span>

* <span data-ttu-id="518c4-217">No Portal do Azure, localize a folha do aplicativo Web e clique em **Procurar** para exibir o aplicativo no navegador padrão.</span><span class="sxs-lookup"><span data-stu-id="518c4-217">In the Azure Portal, locate the web app blade for your web app, and click **Browse** to view your app in your default browser.</span></span>

* <span data-ttu-id="518c4-218">Abra um navegador e insira a URL do aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="518c4-218">Open a browser and enter the URL for your web app.</span></span> <span data-ttu-id="518c4-219">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="518c4-219">For example:</span></span>

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a><span data-ttu-id="518c4-220">Atualizar o aplicativo Web e publicá-lo novamente</span><span class="sxs-lookup"><span data-stu-id="518c4-220">Update your web app and republish</span></span>

<span data-ttu-id="518c4-221">Depois de fazer alterações no código local, você poderá republicá-lo.</span><span class="sxs-lookup"><span data-stu-id="518c4-221">After you make changes to your local code, you can republish.</span></span>

1.  <span data-ttu-id="518c4-222">No **Gerenciador de Soluções** do Visual Studio, abra o arquivo *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="518c4-222">In **Solution Explorer** of Visual Studio, open the *Startup.cs* file.</span></span>

2.  <span data-ttu-id="518c4-223">No método `Configure`, modifique o método `Response.WriteAsync` para que ele seja exibido da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="518c4-223">In the `Configure` method, modify the `Response.WriteAsync` method so that it appears as follows:</span></span>

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  <span data-ttu-id="518c4-224">Salve as alterações em *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="518c4-224">Save changes to *Startup.cs*.</span></span>

4.  <span data-ttu-id="518c4-225">No **Gerenciador de Soluções**, clique com o botão direito do mouse em **“SampleWebAppDemo” da Solução** e selecione **Confirmar**.</span><span class="sxs-lookup"><span data-stu-id="518c4-225">In **Solution Explorer**, right-click **Solution 'SampleWebAppDemo'** and select **Commit**.</span></span> <span data-ttu-id="518c4-226">O **Team Explorer** será exibido.</span><span class="sxs-lookup"><span data-stu-id="518c4-226">The **Team Explorer** will be displayed.</span></span>

5.  <span data-ttu-id="518c4-227">Escreva uma mensagem de confirmação, como:</span><span class="sxs-lookup"><span data-stu-id="518c4-227">Enter a commit message, such as:</span></span>

    ```none
    Update #2
    ```

6.  <span data-ttu-id="518c4-228">Pressione o botão **Confirmar** para confirmar as alterações do projeto.</span><span class="sxs-lookup"><span data-stu-id="518c4-228">Press the **Commit** button to commit the project changes.</span></span>

7.  <span data-ttu-id="518c4-229">Selecione **Página Inicial** > **Sincronização** > **Ações** > **Enviar por Push**.</span><span class="sxs-lookup"><span data-stu-id="518c4-229">Select **Home** > **Sync** > **Actions** > **Push**.</span></span>

>[!NOTE]
><span data-ttu-id="518c4-230">Como alternativa, você pode enviar as alterações por push na **Janela Comando** abrindo a **Janela Comando**, alterando para o diretório do projeto e inserindo um comando do Git.</span><span class="sxs-lookup"><span data-stu-id="518c4-230">As an alternative, you can push your changes from the **Command Window** by opening the **Command Window**, changing to your project directory, and entering a git command.</span></span> <span data-ttu-id="518c4-231">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="518c4-231">For example:</span></span>
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a><span data-ttu-id="518c4-232">Exibir o aplicativo Web atualizado no Azure</span><span class="sxs-lookup"><span data-stu-id="518c4-232">View the updated web app in Azure</span></span>

<span data-ttu-id="518c4-233">Exiba o aplicativo Web atualizado selecionando **Procurar** na folha do aplicativo Web no Portal do Azure ou abrindo um navegador e inserindo a URL do aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="518c4-233">View your updated web app by selecting **Browse** from the web app blade in the Azure Portal or by opening a browser and entering the URL for your web app.</span></span> <span data-ttu-id="518c4-234">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="518c4-234">For example:</span></span>

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a><span data-ttu-id="518c4-235">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="518c4-235">Additional Resources</span></span>

* [<span data-ttu-id="518c4-236">Publicação e implantação</span><span class="sxs-lookup"><span data-stu-id="518c4-236">Publishing and Deployment</span></span>](index.md)

* [<span data-ttu-id="518c4-237">Kudu do projeto</span><span class="sxs-lookup"><span data-stu-id="518c4-237">Project Kudu</span></span>](https://github.com/projectkudu/kudu/wiki)

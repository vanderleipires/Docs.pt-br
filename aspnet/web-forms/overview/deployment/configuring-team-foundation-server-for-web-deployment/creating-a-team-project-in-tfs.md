---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Criando um projeto de equipe no TFS | Microsoft Docs
author: jrjlee
description: Este tópico descreve como criar um novo projeto de equipe no Team Foundation Server (TFS) 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 2c3b30cac408f47d7d15ae7456f0744219506c85
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397555"
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="7d996-103">Criando um projeto de equipe no TFS</span><span class="sxs-lookup"><span data-stu-id="7d996-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="7d996-104">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="7d996-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="7d996-105">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="7d996-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="7d996-106">Este tópico descreve como criar um novo projeto de equipe no Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="7d996-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="7d996-107">Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7d996-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="7d996-108">Visão geral da tarefa</span><span class="sxs-lookup"><span data-stu-id="7d996-108">Task Overview</span></span>

<span data-ttu-id="7d996-109">Para provisionar e usar um novo projeto de equipe no TFS, você precisa concluir estas etapas de alto nível:</span><span class="sxs-lookup"><span data-stu-id="7d996-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="7d996-110">Conceder permissões ao usuário que irá criar um novo projeto de equipe.</span><span class="sxs-lookup"><span data-stu-id="7d996-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="7d996-111">Crie o projeto de equipe.</span><span class="sxs-lookup"><span data-stu-id="7d996-111">Create the team project.</span></span>
- <span data-ttu-id="7d996-112">Conceder permissões a membros da equipe que funcionarão no projeto.</span><span class="sxs-lookup"><span data-stu-id="7d996-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="7d996-113">Fazer check-in algum conteúdo.</span><span class="sxs-lookup"><span data-stu-id="7d996-113">Check in some content.</span></span>

<span data-ttu-id="7d996-114">Este tópico mostra como executar esses procedimentos, e ele identificará os usuários e funções de trabalho que provavelmente serão responsáveis por cada procedimento.</span><span class="sxs-lookup"><span data-stu-id="7d996-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="7d996-115">Lembre-se de que, dependendo da estrutura da sua organização, cada uma dessas tarefas pode ser a responsabilidade de uma pessoa diferente.</span><span class="sxs-lookup"><span data-stu-id="7d996-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="7d996-116">As tarefas e instruções passo a passo neste tópico pressupõem que você instalou e configurou o TFS e que você criou uma coleção de projetos de equipe como parte do processo de configuração.</span><span class="sxs-lookup"><span data-stu-id="7d996-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="7d996-117">Para obter mais informações sobre essas suposições e para obter mais informações em segundo plano no cenário, consulte [configurar um servidor de compilação do TFS para implantação da Web](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="7d996-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="7d996-118">Conceder permissões para o criador do projeto de equipe</span><span class="sxs-lookup"><span data-stu-id="7d996-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="7d996-119">Para criar um novo projeto de equipe, você precisará dessas permissões:</span><span class="sxs-lookup"><span data-stu-id="7d996-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="7d996-120">Você deve ter o **criar novos projetos** permissão na camada de aplicativo do TFS.</span><span class="sxs-lookup"><span data-stu-id="7d996-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="7d996-121">Você normalmente concede essa permissão, adicionando usuários para o **administradores da coleção de projeto** grupo do TFS.</span><span class="sxs-lookup"><span data-stu-id="7d996-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="7d996-122">O **administradores do Team Foundation** grupo global também inclui essa permissão.</span><span class="sxs-lookup"><span data-stu-id="7d996-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="7d996-123">Você deve ter permissão para criar novos sites de equipe dentro da coleção de site do SharePoint que corresponde à coleção de projeto de equipe do TFS.</span><span class="sxs-lookup"><span data-stu-id="7d996-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="7d996-124">Você normalmente concede essa permissão, adicionando o usuário a um grupo do SharePoint com **controle total** conjunto de sites de direitos no SharePoint.</span><span class="sxs-lookup"><span data-stu-id="7d996-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="7d996-125">Se você estiver usando recursos do SQL Server Reporting Services, você deve ser um membro do **Team Foundation Content Manager** função no Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="7d996-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="7d996-126">Quem executa esses procedimentos?</span><span class="sxs-lookup"><span data-stu-id="7d996-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="7d996-127">Normalmente, a pessoa ou grupo que administra a implantação de TFS também executa esses procedimentos.</span><span class="sxs-lookup"><span data-stu-id="7d996-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="7d996-128">Como esse é um conjunto altamente privilegiado de permissões, novos projetos de equipe normalmente são criados por um pequeno subconjunto de usuários com a responsabilidade de administrar uma implantação do TFS.</span><span class="sxs-lookup"><span data-stu-id="7d996-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="7d996-129">Os desenvolvedores não normalmente recebem as permissões necessárias para criar novos projetos de equipe.</span><span class="sxs-lookup"><span data-stu-id="7d996-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="7d996-130">Conceder permissões no TFS</span><span class="sxs-lookup"><span data-stu-id="7d996-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="7d996-131">Se você quiser permitir que um usuário criar novos projetos de equipe, a primeira tarefa de alto nível é adicionar o usuário para o **administradores da coleção de projeto** grupo para a coleção de projeto de equipe.</span><span class="sxs-lookup"><span data-stu-id="7d996-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="7d996-132">**Para adicionar um usuário ao grupo Administradores de coleção de projeto**</span><span class="sxs-lookup"><span data-stu-id="7d996-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="7d996-133">No servidor do TFS, sobre o **inicie** , aponte para **todos os programas**, clique em **Microsoft Team Foundation Server 2010**e, em seguida, clique em **Team Foundation Console de administração**.</span><span class="sxs-lookup"><span data-stu-id="7d996-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="7d996-134">Na exibição de árvore de navegação, expanda **camada de aplicativo**e, em seguida, clique em **coleções de projetos de equipe**.</span><span class="sxs-lookup"><span data-stu-id="7d996-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="7d996-135">No **coleções de projetos de equipe** painel, selecione o projeto de equipe coleção que você deseja gerenciar.</span><span class="sxs-lookup"><span data-stu-id="7d996-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="7d996-136">Sobre o **gerais** , clique em **associação de grupo**.</span><span class="sxs-lookup"><span data-stu-id="7d996-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="7d996-137">No **grupos globais** caixa de diálogo, selecione o **administradores de coleção de projetos** agrupar e, em seguida, clique em **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="7d996-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="7d996-138">No **propriedades de grupo do Team Foundation Server** caixa de diálogo, selecione **usuário do Windows ou grupo**e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="7d996-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="7d996-139">No **selecionar usuários, computadores ou grupos** caixa de diálogo, digite o nome de usuário do usuário que você deseja ser capaz de criar novos projetos de equipe, clique em **verificar nomes**e, em seguida, clique em **Okey** .</span><span class="sxs-lookup"><span data-stu-id="7d996-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="7d996-140">No **propriedades de grupo do Team Foundation Server** caixa de diálogo, clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="7d996-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="7d996-141">No **grupos globais** caixa de diálogo, clique em **fechar**.</span><span class="sxs-lookup"><span data-stu-id="7d996-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="7d996-142">Conceder permissões no SharePoint Services</span><span class="sxs-lookup"><span data-stu-id="7d996-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="7d996-143">Em seguida, você precisa dar permissão ao usuário para criar novos sites de equipe na coleção de sites do SharePoint que corresponde à sua coleção de projeto de equipe do TFS.</span><span class="sxs-lookup"><span data-stu-id="7d996-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="7d996-144">**Para conceder permissões de controle total sobre o conjunto de sites do SharePoint**</span><span class="sxs-lookup"><span data-stu-id="7d996-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="7d996-145">No Console de administração do Team Foundation Server, sobre o **coleções de projetos de equipe** , selecione a coleção de projetos de equipe que você deseja gerenciar.</span><span class="sxs-lookup"><span data-stu-id="7d996-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="7d996-146">Sobre o **Site do SharePoint** guia, observe o valor da **local atual do Site padrão** URL.</span><span class="sxs-lookup"><span data-stu-id="7d996-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="7d996-147">Abra o Internet Explorer e, em seguida, vá para a URL que você anotou na etapa 2.</span><span class="sxs-lookup"><span data-stu-id="7d996-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7d996-148">Se você não estiver conectado ao Windows como o usuário que criou a coleção de projetos de equipe, você precisará entrar no SharePoint como esse usuário para continuar.</span><span class="sxs-lookup"><span data-stu-id="7d996-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="7d996-149">Sobre o **ações do Site** menu, clique em **configurações de Site**.</span><span class="sxs-lookup"><span data-stu-id="7d996-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="7d996-150">Sobre o **configurações de Site** página, em **usuários e permissões**, clique em **pessoas e grupos**.</span><span class="sxs-lookup"><span data-stu-id="7d996-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="7d996-151">No painel de navegação à esquerda, clique em **grupos**.</span><span class="sxs-lookup"><span data-stu-id="7d996-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="7d996-152">Sobre o **pessoas e grupos: todos os grupos** , clique em **configurar grupos para esse Site**.</span><span class="sxs-lookup"><span data-stu-id="7d996-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="7d996-153">Você pode receber um <strong>HTTP 404 não encontrado</strong> erro devido a um bug de codificação duplo do HTTP.</span><span class="sxs-lookup"><span data-stu-id="7d996-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="7d996-154">Se isso ocorrer, substitua a URL com este:</span><span class="sxs-lookup"><span data-stu-id="7d996-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="7d996-155">`[site_collection_URL]/_layouts/permsetup.aspx` Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7d996-155">`[site_collection_URL]/_layouts/permsetup.aspx` For example:</span></span>  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. <span data-ttu-id="7d996-156">Sobre o **configurar grupos para esse Site** página, adicione o usuário que criarão projetos de equipe para o **proprietários** de grupo e, em seguida, clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="7d996-156">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="7d996-157">Para obter mais informações sobre como habilitar usuários criar novos projetos de equipe em uma coleção de projeto de equipe, consulte [definir permissões de administrador para coleções de projetos de equipe](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="7d996-157">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="7d996-158">Criar um novo projeto de equipe e adicionar usuários</span><span class="sxs-lookup"><span data-stu-id="7d996-158">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="7d996-159">Quando você tiver as permissões necessárias, você pode usar o **Team Explorer** janela no Visual Studio 2010 para criar um novo projeto de equipe.</span><span class="sxs-lookup"><span data-stu-id="7d996-159">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="7d996-160">Essa abordagem oferece um assistente que coleta todas as informações necessárias e executa as tarefas necessárias no TFS, SharePoint e SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="7d996-160">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="7d996-161">Você também precisará conceder permissões em um novo projeto de equipe aos membros da equipe do desenvolvedor, a fim de habilitá-los adicionar e modificar o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="7d996-161">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="7d996-162">Quem executa esses procedimentos?</span><span class="sxs-lookup"><span data-stu-id="7d996-162">Who Performs These Procedures?</span></span>

<span data-ttu-id="7d996-163">Normalmente, um administrador do TFS ou um líder de equipe do desenvolvedor executa esses procedimentos.</span><span class="sxs-lookup"><span data-stu-id="7d996-163">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="7d996-164">Criar um novo projeto de equipe</span><span class="sxs-lookup"><span data-stu-id="7d996-164">Create a New Team Project</span></span>

<span data-ttu-id="7d996-165">O procedimento a seguir descreve como criar um novo projeto de equipe no TFS 2010.</span><span class="sxs-lookup"><span data-stu-id="7d996-165">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="7d996-166">**Para criar um novo projeto de equipe**</span><span class="sxs-lookup"><span data-stu-id="7d996-166">**To create a new team project**</span></span>

1. <span data-ttu-id="7d996-167">Sobre o **iniciar** , aponte para **todos os programas**, clique em **Microsoft Visual Studio 2010**, clique com botão direito **Microsoft Visual Studio 2010**, e, em seguida, clique em **executar como administrador**.</span><span class="sxs-lookup"><span data-stu-id="7d996-167">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7d996-168">Se você não executar o Visual Studio 2010 como administrador, o Assistente do novo projeto de equipe falhará na última etapa.</span><span class="sxs-lookup"><span data-stu-id="7d996-168">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="7d996-169">Se o **User Account Control** caixa de diálogo for exibida, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="7d996-169">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="7d996-170">No Visual Studio, sobre o **Team** menu, clique em **conectar ao Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="7d996-170">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7d996-171">Se você já tiver configurado uma conexão a um servidor TFS, você poderá omitir as etapas 4 a 7.</span><span class="sxs-lookup"><span data-stu-id="7d996-171">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="7d996-172">No **Conexão ao projeto de equipe** caixa de diálogo, clique em **servidores**.</span><span class="sxs-lookup"><span data-stu-id="7d996-172">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="7d996-173">No **Adicionar/remover Team Foundation Server** caixa de diálogo, clique em **Add**.</span><span class="sxs-lookup"><span data-stu-id="7d996-173">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="7d996-174">No **adicionar Team Foundation Server** caixa de diálogo, forneça os detalhes da sua instância do TFS e, em seguida, clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="7d996-174">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="7d996-175">No **Adicionar/remover Team Foundation Server** caixa de diálogo, clique em **fechar**.</span><span class="sxs-lookup"><span data-stu-id="7d996-175">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="7d996-176">No **conectar-se ao projeto de equipe** caixa de diálogo, selecione a instância do TFS que você deseja se conectar, selecione a equipe de projeto coleção que você deseja adicionar a e, em seguida, clique em **Connect**.</span><span class="sxs-lookup"><span data-stu-id="7d996-176">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="7d996-177">No **Team Explorer** janela, o botão direito do mouse, a equipe de coleção de projeto e, em seguida, clique em **novo projeto de equipe**.</span><span class="sxs-lookup"><span data-stu-id="7d996-177">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="7d996-178">No **novo projeto de equipe** caixa de diálogo, forneça um nome e uma descrição para o projeto de equipe e, em seguida, clique em **próxima**.</span><span class="sxs-lookup"><span data-stu-id="7d996-178">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7d996-179">Se seu projeto de equipe contiver espaços, você pode enfrentar alguns problemas quando você chega a usar a ferramenta de implantação do Internet Information Services (IIS) da Web (implantação da Web) para implantar pacotes do caminho de saída.</span><span class="sxs-lookup"><span data-stu-id="7d996-179">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="7d996-180">Espaços no caminho podem fazer muito mais difícil executar comandos de implantação da Web.</span><span class="sxs-lookup"><span data-stu-id="7d996-180">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="7d996-181">Sobre o **selecione um modelo de processo** , selecione o modelo de processo que você deseja usar para gerenciar o processo de desenvolvimento e, em seguida, clique em **próxima**.</span><span class="sxs-lookup"><span data-stu-id="7d996-181">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="7d996-182">Para obter mais informações sobre modelos de processo do TFS, consulte [modelos de processo e ferramentas](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="7d996-182">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="7d996-183">Sobre o **configurações de Site de equipe** página, deixe as configurações padrão inalteradas e, em seguida, clique em **próxima**.</span><span class="sxs-lookup"><span data-stu-id="7d996-183">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="7d996-184">Essa configuração cria ou identifica um site de equipe do SharePoint que está associado com o projeto de equipe do TFS.</span><span class="sxs-lookup"><span data-stu-id="7d996-184">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="7d996-185">Sua equipe de desenvolvimento pode usar este site para gerenciar a documentação, participar de discussões, criar páginas wiki e realizar várias outras tarefas que não estão relacionadas ao código.</span><span class="sxs-lookup"><span data-stu-id="7d996-185">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="7d996-186">Para obter mais informações, consulte [interações entre produtos do SharePoint e o Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="7d996-186">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="7d996-187">Sobre o **especificar configurações de controle do código-fonte** página, deixe as configurações padrão inalteradas e, em seguida, clique em **próxima**.</span><span class="sxs-lookup"><span data-stu-id="7d996-187">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="7d996-188">Esta configuração identifica ou cria o local na hierarquia de pastas TFS que atuará como uma pasta raiz para o seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="7d996-188">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="7d996-189">Sobre o **Confirmar configurações de projeto de equipe** , clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="7d996-189">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="7d996-190">Quando o novo projeto de equipe é criado com êxito, diante a **projeto de equipe criado** , clique em **fechar**.</span><span class="sxs-lookup"><span data-stu-id="7d996-190">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="7d996-191">Adicionar usuários a um projeto de equipe</span><span class="sxs-lookup"><span data-stu-id="7d996-191">Add Users to a Team Project</span></span>

<span data-ttu-id="7d996-192">Agora que você criou um novo projeto de equipe, você pode conceder permissões aos usuários para que eles possam começar a adicionar e colaborar no conteúdo.</span><span class="sxs-lookup"><span data-stu-id="7d996-192">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="7d996-193">**Para adicionar usuários a um projeto de equipe**</span><span class="sxs-lookup"><span data-stu-id="7d996-193">**To add users to a team project**</span></span>

1. <span data-ttu-id="7d996-194">No Visual Studio 2010, nos **Team Explorer** janela, o projeto de equipe com o botão direito, aponte para **configurações de projeto de equipe**e, em seguida, clique em **associação de grupo**.</span><span class="sxs-lookup"><span data-stu-id="7d996-194">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="7d996-195">Para habilitar um usuário adicionar, modificar e remover o código sob controle do código-fonte, você precisará adicioná-lo para o **colaboradores** grupo.</span><span class="sxs-lookup"><span data-stu-id="7d996-195">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="7d996-196">No **grupos de projetos** caixa de diálogo, selecione o **colaboradores** agrupar e, em seguida, clique em **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="7d996-196">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="7d996-197">No **propriedades de grupo do Team Foundation Server** caixa de diálogo, selecione **usuário do Windows ou grupo**e, em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="7d996-197">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="7d996-198">No **selecionar usuários, computadores ou grupos** caixa de diálogo, digite o nome de usuário do usuário que você deseja adicionar ao projeto de equipe, clique em **verificar nomes**e, em seguida, clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="7d996-198">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="7d996-199">No **propriedades de grupo do Team Foundation Server** caixa de diálogo, clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="7d996-199">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="7d996-200">No **grupos de projetos** caixa de diálogo, clique em **fechar**.</span><span class="sxs-lookup"><span data-stu-id="7d996-200">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="7d996-201">Conclusão</span><span class="sxs-lookup"><span data-stu-id="7d996-201">Conclusion</span></span>

<span data-ttu-id="7d996-202">Neste ponto, seu novo projeto de equipe está pronto para uso, e sua equipe de desenvolvedores pode iniciar a adição de conteúdo e colaborar em processo de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="7d996-202">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="7d996-203">Próximo tópico [adicionando conteúdo ao controle do código-fonte](adding-content-to-source-control.md), descreve como adicionar conteúdo ao controle de origem.</span><span class="sxs-lookup"><span data-stu-id="7d996-203">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="7d996-204">Leitura adicional</span><span class="sxs-lookup"><span data-stu-id="7d996-204">Further Reading</span></span>

<span data-ttu-id="7d996-205">Para obter mais ampla orientação sobre como criar projetos de equipe no TFS, consulte [criar um projeto de equipe](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="7d996-205">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="7d996-206">Para obter mais informações sobre como habilitar usuários criar novos projetos de equipe em uma coleção de projeto de equipe, consulte [definir permissões de administrador para coleções de projetos de equipe](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="7d996-206">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="7d996-207">Para obter mais informações sobre como adicionar usuários a projetos de equipe, consulte [adicionar usuários a projetos de equipe](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="7d996-207">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7d996-208">[Anterior](configuring-team-foundation-server-for-web-deployment.md)
> [Próximo](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="7d996-208">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>

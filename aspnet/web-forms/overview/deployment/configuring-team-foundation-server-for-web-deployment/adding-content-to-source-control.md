---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Adicionar conteúdo ao controle do código-fonte | Microsoft Docs
author: jrjlee
description: Este tópico explica como adicionar conteúdo ao controle do código-fonte no Team Foundation Server (TFS) 2010. Descreve como adicionar soluções e projetos para um projeto de equipe...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: c9c3a506d2745a6793661453a293732429d3e46e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890428"
---
<a name="adding-content-to-source-control"></a><span data-ttu-id="b4c82-104">Adicionar conteúdo ao controle de origem</span><span class="sxs-lookup"><span data-stu-id="b4c82-104">Adding Content to Source Control</span></span>
====================
<span data-ttu-id="b4c82-105">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="b4c82-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="b4c82-106">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="b4c82-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="b4c82-107">Este tópico explica como adicionar conteúdo ao controle do código-fonte no Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="b4c82-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="b4c82-108">Descreve como adicionar soluções e projetos para um projeto de equipe no TFS e explica como adicionar dependências externas como estruturas ou assemblies ao controle de origem.</span><span class="sxs-lookup"><span data-stu-id="b4c82-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>


<span data-ttu-id="b4c82-109">Este tópico faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b4c82-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="b4c82-110">Visão geral da tarefa</span><span class="sxs-lookup"><span data-stu-id="b4c82-110">Task Overview</span></span>

<span data-ttu-id="b4c82-111">Na maioria dos casos, cada membro da equipe do desenvolvedor deve ser capaz de adicionar conteúdo ao controle de origem.</span><span class="sxs-lookup"><span data-stu-id="b4c82-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="b4c82-112">Para adicionar uma solução ao controle de origem no TFS, você precisará concluir essas etapas de alto nível:</span><span class="sxs-lookup"><span data-stu-id="b4c82-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="b4c82-113">Conecte a um projeto de equipe.</span><span class="sxs-lookup"><span data-stu-id="b4c82-113">Connect to a team project.</span></span>
- <span data-ttu-id="b4c82-114">Mapeie a estrutura de pastas do projeto de equipe no servidor para uma estrutura de pastas no computador local.</span><span class="sxs-lookup"><span data-stu-id="b4c82-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="b4c82-115">Adicione a solução e seu conteúdo ao controle de origem.</span><span class="sxs-lookup"><span data-stu-id="b4c82-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="b4c82-116">Adicione quaisquer dependências externas ao controle de origem.</span><span class="sxs-lookup"><span data-stu-id="b4c82-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="b4c82-117">Neste tópico mostram como executar esses procedimentos.</span><span class="sxs-lookup"><span data-stu-id="b4c82-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="b4c82-118">As tarefas e instruções passo a passo neste tópico pressupõem que você já criou um novo projeto de equipe do TFS para gerenciar o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="b4c82-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="b4c82-119">Para obter mais informações sobre como criar um novo projeto de equipe, consulte [criando um projeto de equipe no TFS](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="b4c82-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="b4c82-120">Quem executa esses procedimentos?</span><span class="sxs-lookup"><span data-stu-id="b4c82-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="b4c82-121">Na maioria dos casos, cada membro da equipe do desenvolvedor deve ser capaz de adicionar e modificar o conteúdo dentro de projetos de equipe específico.</span><span class="sxs-lookup"><span data-stu-id="b4c82-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="b4c82-122">Conectar a um projeto de equipe e criar um mapeamento de pasta</span><span class="sxs-lookup"><span data-stu-id="b4c82-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="b4c82-123">Antes de adicionar conteúdo ao controle de origem, você precisa se conectar a um projeto de equipe e criar um mapeamento entre a estrutura de pastas no servidor e o sistema de arquivos em seu computador local.</span><span class="sxs-lookup"><span data-stu-id="b4c82-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="b4c82-124">**Para se conectar a um projeto de equipe e mapear um caminho de local**</span><span class="sxs-lookup"><span data-stu-id="b4c82-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="b4c82-125">Em sua estação de trabalho do desenvolvedor, abra o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="b4c82-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="b4c82-126">No Visual Studio, no **equipe** menu, clique em **conectar ao Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b4c82-127">Se você já tiver configurado uma conexão a um servidor do TFS, você poderá omitir as etapas 3 a 6.</span><span class="sxs-lookup"><span data-stu-id="b4c82-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="b4c82-128">No **Conexão ao projeto de equipe** caixa de diálogo, clique em **servidores**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="b4c82-129">No **Adicionar/remover Team Foundation Server** caixa de diálogo, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="b4c82-130">No **adicionar Team Foundation Server** caixa de diálogo, forneça os detalhes de sua instância do TFS e, em seguida, clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="b4c82-131">No **Adicionar/remover Team Foundation Server** caixa de diálogo, clique em **fechar**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="b4c82-132">No **conectar-se ao projeto de equipe** caixa de diálogo, selecione a instância do TFS que deseja se conectar, selecione a equipe de projeto coleção, selecione o projeto de equipe que você deseja adicionar a e clique **conectar**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="b4c82-133">No **Team Explorer** janela, expanda seu projeto de equipe e, em seguida, clique duas vezes em **controle de origem**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="b4c82-134">Sobre o **Gerenciador de controle do código-fonte** , clique em **não mapeado**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="b4c82-135">No **mapa** na caixa de **pasta Local** caixa, navegue até (ou crie) uma pasta local para agir como a pasta raiz do projeto de equipe e, em seguida, clique em **mapa**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="b4c82-136">Quando você for solicitado a baixar arquivos de origem, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="b4c82-137">Neste ponto, você mapeou na pasta do servidor para o projeto de equipe para uma pasta local em sua estação de trabalho do desenvolvedor.</span><span class="sxs-lookup"><span data-stu-id="b4c82-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="b4c82-138">Você também baixar todo o conteúdo existente do projeto de equipe para sua estrutura de pasta local.</span><span class="sxs-lookup"><span data-stu-id="b4c82-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="b4c82-139">Agora, você pode começar a adicionar seus próprios dados ao controle de origem.</span><span class="sxs-lookup"><span data-stu-id="b4c82-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="b4c82-140">Adicionar soluções e projetos ao controle de origem</span><span class="sxs-lookup"><span data-stu-id="b4c82-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="b4c82-141">Para adicionar soluções e projetos ao controle de origem, primeiro será necessário movê-los para a pasta mapeada para o projeto de equipe em seu computador local.</span><span class="sxs-lookup"><span data-stu-id="b4c82-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="b4c82-142">Em seguida, você pode verificar no conteúdo para sincronizar suas inclusões com o servidor.</span><span class="sxs-lookup"><span data-stu-id="b4c82-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="b4c82-143">**Para adicionar projetos ao controle de origem**</span><span class="sxs-lookup"><span data-stu-id="b4c82-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="b4c82-144">Em sua estação de trabalho do desenvolvedor, mova seus projetos e soluções em um local apropriado dentro da estrutura de pasta mapeada para o projeto de equipe.</span><span class="sxs-lookup"><span data-stu-id="b4c82-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b4c82-145">Muitas organizações terão uma abordagem preferencial como projetos e soluções devem ser organizados em controle de origem.</span><span class="sxs-lookup"><span data-stu-id="b4c82-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="b4c82-146">Para obter orientação sobre como pastas de estrutura, consulte [como: controle de pastas de origem sua estrutura no Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="b4c82-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="b4c82-147">Abra a solução no Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="b4c82-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="b4c82-148">No **Solution Explorer** janela, clique com botão direito a solução e, em seguida, clique em **adicionar solução ao controle de origem**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="b4c82-149">Em alguns casos, dependendo de como sua organização gosta de conteúdo da estrutura no TFS, talvez seja necessário adicionar projetos ao controle de origem individualmente para fornecer um controle mais refinado sobre como o seu código-fonte é organizado.</span><span class="sxs-lookup"><span data-stu-id="b4c82-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="b4c82-150">Verifique o **Gerenciador de controle do código-fonte** guia exibe o conteúdo que você adicionou na estrutura de pastas do servidor para o projeto de equipe.</span><span class="sxs-lookup"><span data-stu-id="b4c82-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="b4c82-151">O **Gerenciador de controle do código-fonte** guia exibe seu conteúdo com nenhum outro aviso porque você adicionou a solução em uma pasta mapeada no sistema de arquivos local.</span><span class="sxs-lookup"><span data-stu-id="b4c82-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="b4c82-152">Se sua solução estava em um local não mapeado, você será solicitado para especificar locais de pasta no TFS e o sistema de arquivos local.</span><span class="sxs-lookup"><span data-stu-id="b4c82-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="b4c82-153">No **Gerenciador de controle do código-fonte** guia o **pastas** painel, clique o projeto de equipe (por exemplo, **ContactManager**) e, em seguida, clique em **Check-In As alterações pendentes**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="b4c82-154">No **Check-In – arquivos de origem** caixa de diálogo, digite um comentário e, em seguida, clique em **Check-In**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="b4c82-155">Neste ponto, você adicionou sua solução ao controle de origem no TFS.</span><span class="sxs-lookup"><span data-stu-id="b4c82-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="b4c82-156">Adicionar dependências externas ao controle de origem</span><span class="sxs-lookup"><span data-stu-id="b4c82-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="b4c82-157">Quando você adiciona um projeto ou solução ao controle de origem, todos os arquivos e pastas dentro de seu projeto ou solução também serão adicionadas.</span><span class="sxs-lookup"><span data-stu-id="b4c82-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="b4c82-158">No entanto, em muitos casos, projetos e soluções também dependem de dependências externas, como assemblies locais, para funcionar corretamente.</span><span class="sxs-lookup"><span data-stu-id="b4c82-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="b4c82-159">Você precisa adicionar todos esses recursos ao controle de origem para permitir que o Team Build e outros membros da equipe de desenvolvedor compilar seu código com êxito.</span><span class="sxs-lookup"><span data-stu-id="b4c82-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="b4c82-160">Por exemplo, a estrutura de pasta para a solução de exemplo Contact Manager inclui uma pasta chamada pacotes.</span><span class="sxs-lookup"><span data-stu-id="b4c82-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="b4c82-161">Contém o assembly e vários recursos de suporte para o ADO.NET Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="b4c82-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="b4c82-162">A pasta de pacotes não é parte da solução de Gerenciador de contato, mas a solução não será criado com êxito sem eles.</span><span class="sxs-lookup"><span data-stu-id="b4c82-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="b4c82-163">Para habilitar o Team Build compilar a solução, você precisa adicionar a pasta pacotes ao controle de origem.</span><span class="sxs-lookup"><span data-stu-id="b4c82-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="b4c82-164">A inclusão de uma pasta de pacotes é típica do que acontece quando você adiciona o Entity Framework ou recursos semelhantes para a solução usando a extensão do NuGet para Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="b4c82-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>


<span data-ttu-id="b4c82-165">**Para adicionar o conteúdo do projeto ao controle de origem**</span><span class="sxs-lookup"><span data-stu-id="b4c82-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="b4c82-166">Certifique-se de que os itens que você deseja adicionar (por exemplo, a pasta de pacotes) estão em um local apropriado dentro de uma pasta mapeada no sistema de arquivos local.</span><span class="sxs-lookup"><span data-stu-id="b4c82-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="b4c82-167">No Visual Studio 2010, no **Team Explorer** janela, expanda seu projeto de equipe e, em seguida, clique duas vezes em **controle de origem**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="b4c82-168">Sobre o **Gerenciador de controle do código-fonte** guia o **pastas** painel, selecione a pasta que contém o item ou itens que você deseja adicionar.</span><span class="sxs-lookup"><span data-stu-id="b4c82-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="b4c82-169">Clique o **adicionar itens à pasta** botão.</span><span class="sxs-lookup"><span data-stu-id="b4c82-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="b4c82-170">No **adicionar ao controle de origem** caixa de diálogo, selecione a pasta ou itens que você deseja adicionar e, em seguida, clique em **próximo**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="b4c82-171">Sobre o **excluídos itens** , selecione todos os itens necessários foram excluídos automaticamente (por exemplo, assemblies) e, em seguida, clique em **incluem itens**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="b4c82-172">Sobre o **itens para adicionar** guia, verifique se todos os arquivos que você deseja incluir são listados e, em seguida, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="b4c82-173">No **Gerenciador de controle do código-fonte** janela, clique no **Check-In** botão.</span><span class="sxs-lookup"><span data-stu-id="b4c82-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="b4c82-174">No **Check-In – arquivos de origem** caixa de diálogo, digite um comentário e, em seguida, clique em **Check-In**.</span><span class="sxs-lookup"><span data-stu-id="b4c82-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="b4c82-175">Neste ponto, você adicionou as dependências externas para sua solução ao controle de origem.</span><span class="sxs-lookup"><span data-stu-id="b4c82-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="b4c82-176">Conclusão</span><span class="sxs-lookup"><span data-stu-id="b4c82-176">Conclusion</span></span>

<span data-ttu-id="b4c82-177">Este tópico descrita como se conectar a um projeto de equipe, mapear uma estrutura de pasta e adicionar conteúdo ao controle de origem.</span><span class="sxs-lookup"><span data-stu-id="b4c82-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="b4c82-178">Para obter mais informações sobre como trabalhar com itens sob controle de origem, consulte [usando o controle de versão](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="b4c82-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="b4c82-179">O próximo tópico, [Configurando um servidor de compilação do TFS para implantação da Web](configuring-a-tfs-build-server-for-web-deployment.md), descreve como preparar um servidor TFS Team Build para compilar e implantar sua solução.</span><span class="sxs-lookup"><span data-stu-id="b4c82-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="b4c82-180">Leitura adicional</span><span class="sxs-lookup"><span data-stu-id="b4c82-180">Further Reading</span></span>

<span data-ttu-id="b4c82-181">Para obter informações mais abrangentes sobre como trabalhar com o controle de origem no TFS, consulte [usando o controle de versão](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="b4c82-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b4c82-182">[Anterior](creating-a-team-project-in-tfs.md)
> [Próximo](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="b4c82-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>

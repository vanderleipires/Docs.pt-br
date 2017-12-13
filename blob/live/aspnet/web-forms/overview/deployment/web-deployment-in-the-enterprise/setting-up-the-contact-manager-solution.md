---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: "Configurar a solução de Gerenciador de contato | Microsoft Docs"
author: jrjlee
description: "Este tópico descreve como baixar e configurar a solução de Gerenciador de contato para ser executada localmente em uma estação de trabalho do desenvolvedor."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 85468949ee61504d6076a191b70a96e8018c67aa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="setting-up-the-contact-manager-solution"></a><span data-ttu-id="ecc77-103">Configurar a solução de Gerenciador de contato</span><span class="sxs-lookup"><span data-stu-id="ecc77-103">Setting Up the Contact Manager Solution</span></span>
====================
<span data-ttu-id="ecc77-104">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="ecc77-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="ecc77-105">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="ecc77-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="ecc77-106">Este tópico descreve como baixar e configurar a solução de Gerenciador de contato para ser executada localmente em uma estação de trabalho do desenvolvedor.</span><span class="sxs-lookup"><span data-stu-id="ecc77-106">This topic describes how to download and configure the Contact Manager solution to run locally on a developer workstation.</span></span>


## <a name="system-requirements"></a><span data-ttu-id="ecc77-107">Requisitos de sistema</span><span class="sxs-lookup"><span data-stu-id="ecc77-107">System Requirements</span></span>

<span data-ttu-id="ecc77-108">Para executar a solução de Contact Manager localmente e executar outras tarefas descritas neste tutorial, você precisará instalar esse software em sua estação de trabalho do desenvolvedor:</span><span class="sxs-lookup"><span data-stu-id="ecc77-108">To run the Contact Manager solution locally and to perform the other tasks described in this tutorial, you'll need to install this software on your developer workstation:</span></span>

- <span data-ttu-id="ecc77-109">Visual Studio 2010 Service Pack 1, Premium ou Ultimate Edition</span><span class="sxs-lookup"><span data-stu-id="ecc77-109">Visual Studio 2010 Service Pack 1, Premium or Ultimate Edition</span></span>
- <span data-ttu-id="ecc77-110">Serviços de informações da Internet (IIS) 7.5 Express</span><span class="sxs-lookup"><span data-stu-id="ecc77-110">Internet Information Services (IIS) 7.5 Express</span></span>
- <span data-ttu-id="ecc77-111">SQL Server Express 2008 R2</span><span class="sxs-lookup"><span data-stu-id="ecc77-111">SQL Server Express 2008 R2</span></span>
- <span data-ttu-id="ecc77-112">IIS Web ferramenta de implantação (implantação da Web) 2.1 ou posterior</span><span class="sxs-lookup"><span data-stu-id="ecc77-112">IIS Web Deployment Tool (Web Deploy) 2.1 or later</span></span>
- <span data-ttu-id="ecc77-113">ASP.NET 4.0</span><span class="sxs-lookup"><span data-stu-id="ecc77-113">ASP.NET 4.0</span></span>
- <span data-ttu-id="ecc77-114">O ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="ecc77-114">ASP.NET MVC 3</span></span>
- <span data-ttu-id="ecc77-115">.NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="ecc77-115">.NET Framework 4</span></span>
- <span data-ttu-id="ecc77-116">.NET Framework 3,5 SP1</span><span class="sxs-lookup"><span data-stu-id="ecc77-116">.NET Framework 3.5 SP1</span></span>

<span data-ttu-id="ecc77-117">Com exceção do Visual Studio 2010, você pode baixar e instalar as versões mais recentes de todos esses produtos e componentes por meio de [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span><span class="sxs-lookup"><span data-stu-id="ecc77-117">With the exception of Visual Studio 2010, you can download and install the latest versions of all of these products and components through the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>

## <a name="download-and-extract-the-solution"></a><span data-ttu-id="ecc77-118">Baixe e extraia a solução</span><span class="sxs-lookup"><span data-stu-id="ecc77-118">Download and Extract the Solution</span></span>

<span data-ttu-id="ecc77-119">Você pode baixar o aplicativo de exemplo do Gerenciador de contato da Galeria de códigos do MSDN [aqui](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span><span class="sxs-lookup"><span data-stu-id="ecc77-119">You can download the Contact Manager sample application from the MSDN Code Gallery [here](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span></span>

## <a name="configure-and-run-the-solution"></a><span data-ttu-id="ecc77-120">Configurar e executar a solução</span><span class="sxs-lookup"><span data-stu-id="ecc77-120">Configure and Run the Solution</span></span>

<span data-ttu-id="ecc77-121">Para configurar e executar a solução de Gerenciador de contato em seu computador local, você precisará executar essas etapas de alto nível:</span><span class="sxs-lookup"><span data-stu-id="ecc77-121">To configure and run the Contact Manager solution on your local machine, you'll need to perform these high-level steps:</span></span>

1. <span data-ttu-id="ecc77-122">Se você não tiver um já, crie um banco de dados de serviços de aplicativo ASP.NET local com os recursos de gerenciamento de associação e funções habilitados.</span><span class="sxs-lookup"><span data-stu-id="ecc77-122">If you don't have one already, create a local ASP.NET application services database with the membership and role management features enabled.</span></span>
2. <span data-ttu-id="ecc77-123">Editar cadeias de caracteres de conexão no *Web. config* arquivos para apontar para sua instância local do SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="ecc77-123">Edit connection strings in the *web.config* files to point to your local SQL Server Express instance.</span></span>
3. <span data-ttu-id="ecc77-124">Execute a solução do Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="ecc77-124">Run the solution from Visual Studio 2010.</span></span>

<span data-ttu-id="ecc77-125">O restante desta seção fornece mais orientações sobre como concluir cada uma dessas tarefas.</span><span class="sxs-lookup"><span data-stu-id="ecc77-125">The remainder of this section provides more guidance on how to complete each of these tasks.</span></span>

<span data-ttu-id="ecc77-126">**Para criar o banco de dados de serviços de aplicativo**</span><span class="sxs-lookup"><span data-stu-id="ecc77-126">**To create the application services database**</span></span>

1. <span data-ttu-id="ecc77-127">Abra um prompt de comando Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="ecc77-127">Open a Visual Studio 2010 command prompt.</span></span> <span data-ttu-id="ecc77-128">Para fazer isso, no **iniciar** , aponte para **todos os programas**, clique em **Microsoft Visual Studio 2010**, clique em **ferramentas do Visual Studio**e, em seguida, Clique em **Prompt de comando do Visual Studio (2010)**.</span><span class="sxs-lookup"><span data-stu-id="ecc77-128">To do this, on the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, click **Visual Studio Tools**, and then click **Visual Studio Command Prompt (2010)**.</span></span>
2. <span data-ttu-id="ecc77-129">No prompt de comando, digite o seguinte comando e pressione Enter:</span><span class="sxs-lookup"><span data-stu-id="ecc77-129">At the command prompt, type this command, and then press Enter:</span></span>

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. <span data-ttu-id="ecc77-130">Use o **– C** para especificar a cadeia de caracteres de conexão para o servidor de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ecc77-130">Use the **–C** switch to specify the connection string for your database server.</span></span>
    2. <span data-ttu-id="ecc77-131">Use o **– um** para especificar o aplicativo de serviços de recursos que você deseja adicionar ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ecc77-131">Use the **–A** switch to specify the application services features you want to add to the database.</span></span> <span data-ttu-id="ecc77-132">Nesse caso, **m** indica que você deseja adicionar suporte para o provedor de associação e **r** indica que você deseja adicionar suporte para o Gerenciador de função.</span><span class="sxs-lookup"><span data-stu-id="ecc77-132">In this case, **m** indicates that you want to add support for the membership provider and **r** indicates that you want to add support for the role manager.</span></span>
    3. <span data-ttu-id="ecc77-133">Use o **– d** para especificar um nome para seu banco de dados de serviços de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ecc77-133">Use the **–d** switch to specify a name for your application services database.</span></span> <span data-ttu-id="ecc77-134">Se esta opção for omitida, o utilitário criará um banco de dados com o nome padrão do **aspnetdb**.</span><span class="sxs-lookup"><span data-stu-id="ecc77-134">If you omit this switch, the utility will create a database with the default name of **aspnetdb**.</span></span>
3. <span data-ttu-id="ecc77-135">Quando o banco de dados foi criado com êxito, o prompt de comando mostrará uma confirmação.</span><span class="sxs-lookup"><span data-stu-id="ecc77-135">When the database has been created successfully, the command prompt will show a confirmation.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="ecc77-136">Para obter mais informações sobre o aspnet\_regsql utilitário, consulte [ferramenta de registro de servidor de SQL do ASP.NET (Aspnet\_regsql.exe)](https://msdn.microsoft.com/en-us/library/ms229862(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="ecc77-136">For more information on the aspnet\_regsql utility, see [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/en-us/library/ms229862(v=vs.100).aspx).</span></span>


<span data-ttu-id="ecc77-137">A próxima etapa é certificar-se de que as cadeias de caracteres de conexão na solução entre em contato com o Gerenciador de ponto para sua instância local do SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="ecc77-137">The next step is to make sure that the connection strings in the Contact Manager solution point to your local instance of SQL Server Express.</span></span>

<span data-ttu-id="ecc77-138">**Para atualizar as cadeias de caracteres de conexão**</span><span class="sxs-lookup"><span data-stu-id="ecc77-138">**To update the connection strings**</span></span>

1. <span data-ttu-id="ecc77-139">Abra a solução de Gerenciador de contato no Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="ecc77-139">Open the Contact Manager solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="ecc77-140">No **Solution Explorer** janela, expanda o **ContactManager.Mvc** de projeto e, em seguida, clique duas vezes o **Web. config** nó.</span><span class="sxs-lookup"><span data-stu-id="ecc77-140">In the **Solution Explorer** window, expand the **ContactManager.Mvc** project, and then double-click the **Web.config** node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ecc77-141">O projeto ContactManager.Mvc inclui dois *Web. config* arquivos.</span><span class="sxs-lookup"><span data-stu-id="ecc77-141">The ContactManager.Mvc project includes two *web.config* files.</span></span> <span data-ttu-id="ecc77-142">Você precisa editar o arquivo de nível de projeto.</span><span class="sxs-lookup"><span data-stu-id="ecc77-142">You need to edit the project-level file.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. <span data-ttu-id="ecc77-143">No **connectionStrings** elemento, verifique se que a cadeia de caracteres de conexão denominada **ApplicationServices** aponta para o banco de dados de serviços de aplicativo ASP.NET local.</span><span class="sxs-lookup"><span data-stu-id="ecc77-143">In the **connectionStrings** element, verify that the connection string named **ApplicationServices** points to your local ASP.NET application services database.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. <span data-ttu-id="ecc77-144">No **Solution Explorer** janela, expanda o **ContactManager.Service** de projeto e, em seguida, clique duas vezes o **Web. config** nó.</span><span class="sxs-lookup"><span data-stu-id="ecc77-144">In the **Solution Explorer** window, expand the **ContactManager.Service** project, and then double-click the **Web.config** node.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. <span data-ttu-id="ecc77-145">No **connectionStrings** elemento na cadeia de conexão denominada **ContactManagerContext**, verifique o **fonte de dados** propriedade for definida como sua instância local do SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="ecc77-145">In the **connectionStrings** element, in the connection string named **ContactManagerContext**, verify that the **Data Source** property is set to your local instance of SQL Server Express.</span></span> <span data-ttu-id="ecc77-146">Você não precisa alterar nada na cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="ecc77-146">You don't need to change anything else in the connection string.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. <span data-ttu-id="ecc77-147">Salve todos os arquivos abertos.</span><span class="sxs-lookup"><span data-stu-id="ecc77-147">Save all open files.</span></span>

<span data-ttu-id="ecc77-148">Você deve agora estar pronto para executar a solução de Gerenciador de contato em seu computador local.</span><span class="sxs-lookup"><span data-stu-id="ecc77-148">You should now be ready to run the Contact Manager solution on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="ecc77-149">Se você seguir estas etapas sem primeiro criar um banco de dados de serviços de aplicativo, o ASP.NET criará o banco de dados na primeira vez que você tentar criar um usuário.</span><span class="sxs-lookup"><span data-stu-id="ecc77-149">If you follow these steps without first creating an application services database, ASP.NET will create the database the first time you attempt to create a user.</span></span> <span data-ttu-id="ecc77-150">No entanto, criar manualmente o banco de dados fornece muito mais controle sobre o conjunto de recursos de serviços de aplicativo que você deseja dar suporte.</span><span class="sxs-lookup"><span data-stu-id="ecc77-150">However, manually creating the database gives you a lot more control over the application services feature set you want to support.</span></span>


<span data-ttu-id="ecc77-151">**Para executar a solução de Gerenciador de contato**</span><span class="sxs-lookup"><span data-stu-id="ecc77-151">**To run the Contact Manager solution**</span></span>

1. <span data-ttu-id="ecc77-152">No Visual Studio 2010, pressione F5.</span><span class="sxs-lookup"><span data-stu-id="ecc77-152">In Visual Studio 2010, press F5.</span></span>
2. <span data-ttu-id="ecc77-153">Internet Explorer é iniciado e solicita a URL do aplicativo entre em contato com o Gerenciador ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="ecc77-153">Internet Explorer starts up and requests the URL of the Contact Manager ASP.NET MVC 3 application.</span></span> <span data-ttu-id="ecc77-154">Por padrão, o aplicativo exibe o **todos os contatos** página.</span><span class="sxs-lookup"><span data-stu-id="ecc77-154">By default, the application displays the **All Contacts** page.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. <span data-ttu-id="ecc77-155">Adicione alguns contatos e, em seguida, verifique se que o aplicativo funciona conforme o esperado.</span><span class="sxs-lookup"><span data-stu-id="ecc77-155">Add a few contacts, and then verify that the application works as expected.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. <span data-ttu-id="ecc77-156">Navegue até `http://localhost:50114/Account/Register` (ajuste a URL se você estiver hospedando o aplicativo em uma porta diferente).</span><span class="sxs-lookup"><span data-stu-id="ecc77-156">Browse to `http://localhost:50114/Account/Register` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="ecc77-157">Adicionar um nome de usuário, o endereço de email e a senha e confirmar que você é capaz de registrar uma conta com êxito.</span><span class="sxs-lookup"><span data-stu-id="ecc77-157">Add a user name, email address, and password, and verify that you're able to register an account successfully.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. <span data-ttu-id="ecc77-158">Navegue até `http://localhost:50114/Account/LogOn` (ajuste a URL se você estiver hospedando o aplicativo em uma porta diferente).</span><span class="sxs-lookup"><span data-stu-id="ecc77-158">Browse to `http://localhost:50114/Account/LogOn` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="ecc77-159">Verifique se que você é capaz de fazer logon usando a conta que você acabou de criar.</span><span class="sxs-lookup"><span data-stu-id="ecc77-159">Verify that you're able to log on using the account you just created.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. <span data-ttu-id="ecc77-160">Feche o Internet Explorer para parar a depuração.</span><span class="sxs-lookup"><span data-stu-id="ecc77-160">Close Internet Explorer to stop debugging.</span></span>

## <a name="conclusion"></a><span data-ttu-id="ecc77-161">Conclusão</span><span class="sxs-lookup"><span data-stu-id="ecc77-161">Conclusion</span></span>

<span data-ttu-id="ecc77-162">Neste ponto, a solução de Gerenciador de contato deve ser totalmente configurada para ser executado em seu computador local.</span><span class="sxs-lookup"><span data-stu-id="ecc77-162">At this point, the Contact Manager solution should be fully configured to run on your local machine.</span></span> <span data-ttu-id="ecc77-163">Você pode usar a solução como referência quando você trabalha com os outros tópicos neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="ecc77-163">You can use the solution as a reference when you work through the other topics in this tutorial.</span></span>

<span data-ttu-id="ecc77-164">O próximo tópico, [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md), explica como você pode usar os arquivos do projeto Microsoft Build Engine (MSBuild) personalizados dentro da solução de Gerenciador de contato para controlar o processo de implantação.</span><span class="sxs-lookup"><span data-stu-id="ecc77-164">The next topic, [Understanding the Project File](understanding-the-project-file.md), explains how you can use the custom Microsoft Build Engine (MSBuild) project files within the Contact Manager solution to control the deployment process.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ecc77-165">[Anterior](the-contact-manager-solution.md)
[Próximo](understanding-the-project-file.md)</span><span class="sxs-lookup"><span data-stu-id="ecc77-165">[Previous](the-contact-manager-solution.md)
[Next](understanding-the-project-file.md)</span></span>

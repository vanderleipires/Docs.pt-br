---
title: Publicar um aplicativo Web ASP.NET Core no Azure usando o Visual Studio
author: rick-anderson
description: "Aprenda como publicar um aplicativo ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/16/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: e8de630c4fa8cff5395f7246b91384d4725e4ca6
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/11/2018
---
<span data-ttu-id="29696-104">/pt-br</span><span class="sxs-lookup"><span data-stu-id="29696-104">/en-us</span></span>

# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="29696-105">Publicar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="29696-105">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="29696-106">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) e [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="29696-106">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="29696-107">Confira [Publicar no Azure do Visual Studio para Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) se você estiver trabalhando em um Mac.</span><span class="sxs-lookup"><span data-stu-id="29696-107">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="29696-108">Configurar</span><span class="sxs-lookup"><span data-stu-id="29696-108">Set up</span></span>

* <span data-ttu-id="29696-109">Abra uma [conta do Azure gratuita](https://aka.ms/K5y5yh) se você não tiver uma.</span><span class="sxs-lookup"><span data-stu-id="29696-109">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="29696-110">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="29696-110">Create a web app</span></span>

<span data-ttu-id="29696-111">Na página inicial do Visual Studio, selecione **Arquivo > Novo > Projeto...**</span><span class="sxs-lookup"><span data-stu-id="29696-111">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![Menu Arquivo](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="29696-113">Preencha a caixa de diálogo **Novo Projeto**:</span><span class="sxs-lookup"><span data-stu-id="29696-113">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="29696-114">No painel esquerdo, selecione **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="29696-114">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="29696-115">No painel central, toque em **Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="29696-115">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="29696-116">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="29696-116">Select **OK**.</span></span>

![Caixa de diálogo Novo Projeto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="29696-118">Na caixa de diálogo **Novo Aplicativo Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="29696-118">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="29696-119">Selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="29696-119">Select **Web Application**.</span></span>
* <span data-ttu-id="29696-120">Selecione **Mudar Autenticação**.</span><span class="sxs-lookup"><span data-stu-id="29696-120">Select **Change Authentication**.</span></span>

![Caixa de diálogo Novo Projeto](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="29696-122">A caixa de diálogo **Mudar Autenticação** é exibida.</span><span class="sxs-lookup"><span data-stu-id="29696-122">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="29696-123">Selecione **Contas de Usuário Individuais**.</span><span class="sxs-lookup"><span data-stu-id="29696-123">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="29696-124">Selecione **OK** para retornar para o **Novo Aplicativo Web do ASP.NET Core**, em seguida, selecione **OK** novamente.</span><span class="sxs-lookup"><span data-stu-id="29696-124">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Caixa de diálogo Nova autenticação da Web do ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="29696-126">O Visual Studio cria a solução.</span><span class="sxs-lookup"><span data-stu-id="29696-126">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="29696-127">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="29696-127">Run the app</span></span>

* <span data-ttu-id="29696-128">Pressione CTRL+F5 para executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="29696-128">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="29696-129">Teste os links **Sobre** e **Contato**.</span><span class="sxs-lookup"><span data-stu-id="29696-129">Test the **About** and **Contact** links.</span></span>

![Aplicativo Web aberto no Microsoft Edge no localhost](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="29696-131">Registrar um usuário</span><span class="sxs-lookup"><span data-stu-id="29696-131">Register a user</span></span>

* <span data-ttu-id="29696-132">Selecione **Registrar** e registre um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="29696-132">Select **Register** and register a new user.</span></span> <span data-ttu-id="29696-133">Você pode usar um endereço de email fictício.</span><span class="sxs-lookup"><span data-stu-id="29696-133">You can use a fictitious email address.</span></span> <span data-ttu-id="29696-134">Ao enviar, a página exibirá o seguinte erro:</span><span class="sxs-lookup"><span data-stu-id="29696-134">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="29696-135">*"Erro interno do servidor: uma operação de banco de dados falhou ao processar a solicitação. Exceção SQL: não é possível abrir o banco de dados. A aplicação de migrações existentes ao contexto do BD do Aplicativo pode resolver esse problema."*</span><span class="sxs-lookup"><span data-stu-id="29696-135">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="29696-136">Selecione **Aplicar Migrações** e, depois que a página for atualizada, atualize a página.</span><span class="sxs-lookup"><span data-stu-id="29696-136">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Erro interno do servidor: uma operação de banco de dados falhou ao processar a solicitação.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="29696-140">O aplicativo exibe o email usado para registrar o novo usuário e um link **Fazer logout**.</span><span class="sxs-lookup"><span data-stu-id="29696-140">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Aplicativo Web aberto no Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="29696-143">Implantar o aplicativo no Azure</span><span class="sxs-lookup"><span data-stu-id="29696-143">Deploy the app to Azure</span></span>

<span data-ttu-id="29696-144">Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e selecione **Publicar...**.</span><span class="sxs-lookup"><span data-stu-id="29696-144">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menu contextual aberto com o link Publicar realçado](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="29696-146">Na caixa de diálogo **Publicar**:</span><span class="sxs-lookup"><span data-stu-id="29696-146">In the **Publish** dialog:</span></span>

* <span data-ttu-id="29696-147">Selecione **Serviço de Aplicativo do Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="29696-147">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="29696-148">Selecione o ícone de engrenagem e, em seguida, **Criar Perfil**.</span><span class="sxs-lookup"><span data-stu-id="29696-148">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="29696-149">Selecione **Criar Perfil**.</span><span class="sxs-lookup"><span data-stu-id="29696-149">Select **Create Profile**.</span></span>

![Caixa de diálogo Publicar](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="29696-151">Criar recursos do Azure</span><span class="sxs-lookup"><span data-stu-id="29696-151">Create Azure resources</span></span>

<span data-ttu-id="29696-152">A caixa de diálogo **Criar Serviço de Aplicativo** será exibida:</span><span class="sxs-lookup"><span data-stu-id="29696-152">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="29696-153">Insira sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="29696-153">Enter your subscription.</span></span>
* <span data-ttu-id="29696-154">Os campos de entrada **Nome do Aplicativo**, **Grupo de Recursos** e **Plano do Serviço de Aplicativo** serão populados.</span><span class="sxs-lookup"><span data-stu-id="29696-154">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="29696-155">Você pode manter esses nomes ou alterá-los.</span><span class="sxs-lookup"><span data-stu-id="29696-155">You can keep these names or change them.</span></span>

![Caixa de diálogo Serviço de Aplicativo](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="29696-157">Selecione a guia **Serviços** para criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="29696-157">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="29696-158">Selecione o ícone verde **+** para criar um novo Banco de Dados SQL</span><span class="sxs-lookup"><span data-stu-id="29696-158">Select the green **+** icon to create a new SQL Database</span></span>

![Novo Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="29696-160">Selecione **Novo...** na caixa de diálogo **Configurar Banco de Dados SQL** para criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="29696-160">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Novo servidor e Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="29696-162">A caixa de diálogo **Configurar o SQL Server** é exibida.</span><span class="sxs-lookup"><span data-stu-id="29696-162">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="29696-163">Insira um nome de usuário do administrador e a senha e, em seguida, selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="29696-163">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="29696-164">Você pode manter o **Nome do Servidor** padrão.</span><span class="sxs-lookup"><span data-stu-id="29696-164">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="29696-165">“admin” não é permitido como o nome de usuário administrador.</span><span class="sxs-lookup"><span data-stu-id="29696-165">"admin" is not allowed as the administrator user name.</span></span>

![Caixa de diálogo Configurar o SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="29696-167">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="29696-167">Select **OK**.</span></span>

<span data-ttu-id="29696-168">O Visual Studio retorna para a caixa de diálogo **Criar Serviço de Aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="29696-168">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="29696-169">Selecione **Criar** na caixa de diálogo **Criar Serviço de Aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="29696-169">Select **Create** on the **Create App Service** dialog.</span></span>

![Caixa de diálogo Configurar o Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="29696-171">O Visual Studio cria o aplicativo Web e o SQL Server no Azure.</span><span class="sxs-lookup"><span data-stu-id="29696-171">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="29696-172">Esta etapa pode levar alguns minutos.</span><span class="sxs-lookup"><span data-stu-id="29696-172">This step can take a few minutes.</span></span> <span data-ttu-id="29696-173">Para obter informações sobre os recursos criados, consulte [Recursos adicionais](#additonal-resources).</span><span class="sxs-lookup"><span data-stu-id="29696-173">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="29696-174">Quando a implantação for concluída, selecione **Configurações**:</span><span class="sxs-lookup"><span data-stu-id="29696-174">When deployment completes, select **Settings**:</span></span>

![Caixa de diálogo Configurar o SQL Server](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="29696-176">Na página **Configurações** da caixa de diálogo **Publicar**:</span><span class="sxs-lookup"><span data-stu-id="29696-176">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="29696-177">Expanda **Bancos de Dados** e marque a opção **Usar esta cadeia de conexão no tempo de execução**.</span><span class="sxs-lookup"><span data-stu-id="29696-177">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="29696-178">Expanda **Migrações do Entity Framework** e marque a opção **Aplicar esta migração durante a publicação**.</span><span class="sxs-lookup"><span data-stu-id="29696-178">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="29696-179">Selecione **Salvar**.</span><span class="sxs-lookup"><span data-stu-id="29696-179">Select **Save**.</span></span> <span data-ttu-id="29696-180">O Visual Studio retorna para a caixa de diálogo **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="29696-180">Visual Studio returns to the **Publish** dialog.</span></span> 

![Caixa de diálogo Publicar: painel Configurações](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="29696-182">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="29696-182">Click **Publish**.</span></span> <span data-ttu-id="29696-183">O Visual Studio publica o aplicativo no Azure.</span><span class="sxs-lookup"><span data-stu-id="29696-183">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="29696-184">Quando a implantação for concluída, o aplicativo será aberto em um navegador.</span><span class="sxs-lookup"><span data-stu-id="29696-184">When the depoyment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="29696-185">Testar o aplicativo no Azure</span><span class="sxs-lookup"><span data-stu-id="29696-185">Test your app in Azure</span></span>

* <span data-ttu-id="29696-186">Teste os links **Sobre** e **Contato**</span><span class="sxs-lookup"><span data-stu-id="29696-186">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="29696-187">Registrar um novo usuário</span><span class="sxs-lookup"><span data-stu-id="29696-187">Register a new user</span></span>

![Aplicativo Web aberto no Microsoft Edge no Serviço de Aplicativo do Azure](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="29696-189">Atualizar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="29696-189">Update the app</span></span>

* <span data-ttu-id="29696-190">Edite a página do Razor *Pages/About.cshtml* e altere seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="29696-190">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="29696-191">Por exemplo, você pode modificar o parágrafo para indicar “Olá, ASP.NET Core!”: [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="29696-191">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="29696-192">Clique com o botão direito do mouse no projeto e selecione **Publicar...** novamente.</span><span class="sxs-lookup"><span data-stu-id="29696-192">Right-click on the project and select **Publish...** again.</span></span>

![Menu contextual aberto com o link Publicar realçado](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="29696-194">Depois que o aplicativo for publicado, verifique se as alterações feitas estão disponíveis no Azure.</span><span class="sxs-lookup"><span data-stu-id="29696-194">After the app is published, verify the changes you made are available on Azure.</span></span>

![Verifique se a tarefa está concluída](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="29696-196">Limpar</span><span class="sxs-lookup"><span data-stu-id="29696-196">Clean up</span></span>

<span data-ttu-id="29696-197">Quando você concluir o teste do aplicativo, acesse o [portal do Azure](https://portal.azure.com/) e exclua o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="29696-197">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="29696-198">Selecione **Grupos de recursos** e, em seguida, selecione o grupo de recursos criado.</span><span class="sxs-lookup"><span data-stu-id="29696-198">Select **Resource groups**, then select the resource group you created.</span></span>

![Portal do Azure: Grupos de Recursos no menu da barra lateral](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="29696-200">Na página **Grupos de recursos**, selecione **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="29696-200">In the **Resource groups** page, select **Delete**.</span></span>

![Portal do Azure: página Grupos de Recursos](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="29696-202">Insira o nome do grupo de recursos e selecione **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="29696-202">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="29696-203">O aplicativo e todos os outros recursos criados neste tutorial agora foram excluídos do Azure.</span><span class="sxs-lookup"><span data-stu-id="29696-203">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="29696-204">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="29696-204">Next steps</span></span>

* [<span data-ttu-id="29696-205">Implantação contínua no Azure com o Visual Studio e o Git</span><span class="sxs-lookup"><span data-stu-id="29696-205">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="29696-206">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="29696-206">Additonal resources</span></span>

* [<span data-ttu-id="29696-207">Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="29696-207">Azure App Service</span></span>](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="29696-208">Grupo de recursos do Azure</span><span class="sxs-lookup"><span data-stu-id="29696-208">Azure resource groups</span></span>](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="29696-209">Banco de Dados SQL do Azure</span><span class="sxs-lookup"><span data-stu-id="29696-209">Azure SQL Database</span></span>](https://docs.microsoft.com/en-us/azure/sql-database/)
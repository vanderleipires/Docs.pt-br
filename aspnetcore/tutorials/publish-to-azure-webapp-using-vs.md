---
title: Publicar um aplicativo Web ASP.NET Core no Azure usando o Visual Studio
author: rick-anderson
description: "Aprenda como publicar um aplicativo ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio."
ms.author: riande
manager: wpickett
ms.date: 12/16/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: dd31e3a9583a0c152e97ae7cf6b215389298a20c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
<span data-ttu-id="c8ccf-103">/pt-br</span><span class="sxs-lookup"><span data-stu-id="c8ccf-103">/en-us</span></span>

# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="c8ccf-104">Publicar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8ccf-104">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="c8ccf-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) e [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="c8ccf-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="c8ccf-106">Confira [Publicar no Azure do Visual Studio para Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) se você estiver trabalhando em um Mac.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-106">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="c8ccf-107">Configurar</span><span class="sxs-lookup"><span data-stu-id="c8ccf-107">Set up</span></span>

* <span data-ttu-id="c8ccf-108">Abra uma [conta do Azure gratuita](https://aka.ms/K5y5yh) se você não tiver uma.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-108">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="c8ccf-109">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="c8ccf-109">Create a web app</span></span>

<span data-ttu-id="c8ccf-110">Na página inicial do Visual Studio, selecione **Arquivo > Novo > Projeto...**</span><span class="sxs-lookup"><span data-stu-id="c8ccf-110">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![Menu Arquivo](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="c8ccf-112">Preencha a caixa de diálogo **Novo Projeto**:</span><span class="sxs-lookup"><span data-stu-id="c8ccf-112">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="c8ccf-113">No painel esquerdo, selecione **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-113">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="c8ccf-114">No painel central, toque em **Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-114">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="c8ccf-115">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-115">Select **OK**.</span></span>

![Caixa de diálogo Novo Projeto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="c8ccf-117">Na caixa de diálogo **Novo Aplicativo Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="c8ccf-117">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="c8ccf-118">Selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-118">Select **Web Application**.</span></span>
* <span data-ttu-id="c8ccf-119">Selecione **Mudar Autenticação**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-119">Select **Change Authentication**.</span></span>

![Caixa de diálogo Novo Projeto](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="c8ccf-121">A caixa de diálogo **Mudar Autenticação** é exibida.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-121">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="c8ccf-122">Selecione **Contas de Usuário Individuais**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-122">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="c8ccf-123">Selecione **OK** para retornar para o **Novo Aplicativo Web do ASP.NET Core**, em seguida, selecione **OK** novamente.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-123">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Caixa de diálogo Nova autenticação da Web do ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="c8ccf-125">O Visual Studio cria a solução.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-125">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="c8ccf-126">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="c8ccf-126">Run the app</span></span>

* <span data-ttu-id="c8ccf-127">Pressione CTRL+F5 para executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-127">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="c8ccf-128">Teste os links **Sobre** e **Contato**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-128">Test the **About** and **Contact** links.</span></span>

![Aplicativo Web aberto no Microsoft Edge no localhost](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="c8ccf-130">Registrar um usuário</span><span class="sxs-lookup"><span data-stu-id="c8ccf-130">Register a user</span></span>

* <span data-ttu-id="c8ccf-131">Selecione **Registrar** e registre um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-131">Select **Register** and register a new user.</span></span> <span data-ttu-id="c8ccf-132">Você pode usar um endereço de email fictício.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-132">You can use a fictitious email address.</span></span> <span data-ttu-id="c8ccf-133">Ao enviar, a página exibirá o seguinte erro:</span><span class="sxs-lookup"><span data-stu-id="c8ccf-133">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="c8ccf-134">*"Erro interno do servidor: uma operação de banco de dados falhou ao processar a solicitação. Exceção SQL: não é possível abrir o banco de dados. A aplicação de migrações existentes ao contexto do BD do Aplicativo pode resolver esse problema."*</span><span class="sxs-lookup"><span data-stu-id="c8ccf-134">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="c8ccf-135">Selecione **Aplicar Migrações** e, depois que a página for atualizada, atualize a página.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-135">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Erro interno do servidor: uma operação de banco de dados falhou ao processar a solicitação.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="c8ccf-139">O aplicativo exibe o email usado para registrar o novo usuário e um link **Fazer logout**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-139">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Aplicativo Web aberto no Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="c8ccf-142">Implantar o aplicativo no Azure</span><span class="sxs-lookup"><span data-stu-id="c8ccf-142">Deploy the app to Azure</span></span>

<span data-ttu-id="c8ccf-143">Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e selecione **Publicar...**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-143">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menu contextual aberto com o link Publicar realçado](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="c8ccf-145">Na caixa de diálogo **Publicar**:</span><span class="sxs-lookup"><span data-stu-id="c8ccf-145">In the **Publish** dialog:</span></span>

* <span data-ttu-id="c8ccf-146">Selecione **Serviço de Aplicativo do Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-146">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="c8ccf-147">Selecione o ícone de engrenagem e, em seguida, **Criar Perfil**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-147">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="c8ccf-148">Selecione **Criar Perfil**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-148">Select **Create Profile**.</span></span>

![Caixa de diálogo Publicar](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="c8ccf-150">Criar recursos do Azure</span><span class="sxs-lookup"><span data-stu-id="c8ccf-150">Create Azure resources</span></span>

<span data-ttu-id="c8ccf-151">A caixa de diálogo **Criar Serviço de Aplicativo** será exibida:</span><span class="sxs-lookup"><span data-stu-id="c8ccf-151">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="c8ccf-152">Insira sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-152">Enter your subscription.</span></span>
* <span data-ttu-id="c8ccf-153">Os campos de entrada **Nome do Aplicativo**, **Grupo de Recursos** e **Plano do Serviço de Aplicativo** serão populados.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-153">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="c8ccf-154">Você pode manter esses nomes ou alterá-los.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-154">You can keep these names or change them.</span></span>

![Caixa de diálogo Serviço de Aplicativo](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="c8ccf-156">Selecione a guia **Serviços** para criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-156">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="c8ccf-157">Selecione o ícone verde **+** para criar um novo Banco de Dados SQL</span><span class="sxs-lookup"><span data-stu-id="c8ccf-157">Select the green **+** icon to create a new SQL Database</span></span>

![Novo Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="c8ccf-159">Selecione **Novo...** na caixa de diálogo **Configurar Banco de Dados SQL** para criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-159">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Novo servidor e Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="c8ccf-161">A caixa de diálogo **Configurar o SQL Server** é exibida.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-161">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="c8ccf-162">Insira um nome de usuário do administrador e a senha e, em seguida, selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-162">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="c8ccf-163">Você pode manter o **Nome do Servidor** padrão.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-163">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="c8ccf-164">“admin” não é permitido como o nome de usuário administrador.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-164">"admin" is not allowed as the administrator user name.</span></span>

![Caixa de diálogo Configurar o SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="c8ccf-166">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-166">Select **OK**.</span></span>

<span data-ttu-id="c8ccf-167">O Visual Studio retorna para a caixa de diálogo **Criar Serviço de Aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-167">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="c8ccf-168">Selecione **Criar** na caixa de diálogo **Criar Serviço de Aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-168">Select **Create** on the **Create App Service** dialog.</span></span>

![Caixa de diálogo Configurar o Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="c8ccf-170">O Visual Studio cria o aplicativo Web e o SQL Server no Azure.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-170">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="c8ccf-171">Esta etapa pode levar alguns minutos.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-171">This step can take a few minutes.</span></span> <span data-ttu-id="c8ccf-172">Para obter informações sobre os recursos criados, consulte [Recursos adicionais](#additonal-resources).</span><span class="sxs-lookup"><span data-stu-id="c8ccf-172">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="c8ccf-173">Quando a implantação for concluída, selecione **Configurações**:</span><span class="sxs-lookup"><span data-stu-id="c8ccf-173">When deployment completes, select **Settings**:</span></span>

![Caixa de diálogo Configurar o SQL Server](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="c8ccf-175">Na página **Configurações** da caixa de diálogo **Publicar**:</span><span class="sxs-lookup"><span data-stu-id="c8ccf-175">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="c8ccf-176">Expanda **Bancos de Dados** e marque a opção **Usar esta cadeia de conexão no tempo de execução**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-176">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="c8ccf-177">Expanda **Migrações do Entity Framework** e marque a opção **Aplicar esta migração durante a publicação**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-177">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="c8ccf-178">Selecione **Salvar**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-178">Select **Save**.</span></span> <span data-ttu-id="c8ccf-179">O Visual Studio retorna para a caixa de diálogo **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-179">Visual Studio returns to the **Publish** dialog.</span></span> 

![Caixa de diálogo Publicar: painel Configurações](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="c8ccf-181">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-181">Click **Publish**.</span></span> <span data-ttu-id="c8ccf-182">O Visual Studio publica o aplicativo no Azure.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-182">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="c8ccf-183">Quando a implantação for concluída, o aplicativo será aberto em um navegador.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-183">When the depoyment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="c8ccf-184">Testar o aplicativo no Azure</span><span class="sxs-lookup"><span data-stu-id="c8ccf-184">Test your app in Azure</span></span>

* <span data-ttu-id="c8ccf-185">Teste os links **Sobre** e **Contato**</span><span class="sxs-lookup"><span data-stu-id="c8ccf-185">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="c8ccf-186">Registrar um novo usuário</span><span class="sxs-lookup"><span data-stu-id="c8ccf-186">Register a new user</span></span>

![Aplicativo Web aberto no Microsoft Edge no Serviço de Aplicativo do Azure](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="c8ccf-188">Atualizar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="c8ccf-188">Update the app</span></span>

* <span data-ttu-id="c8ccf-189">Edite a página do Razor *Pages/About.cshtml* e altere seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-189">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="c8ccf-190">Por exemplo, você pode modificar o parágrafo para indicar “Olá, ASP.NET Core!”: [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="c8ccf-190">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="c8ccf-191">Clique com o botão direito do mouse no projeto e selecione **Publicar...** novamente.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-191">Right-click on the project and select **Publish...** again.</span></span>

![Menu contextual aberto com o link Publicar realçado](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="c8ccf-193">Depois que o aplicativo for publicado, verifique se as alterações feitas estão disponíveis no Azure.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-193">After the app is published, verify the changes you made are available on Azure.</span></span>

![Verifique se a tarefa está concluída](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="c8ccf-195">Limpar</span><span class="sxs-lookup"><span data-stu-id="c8ccf-195">Clean up</span></span>

<span data-ttu-id="c8ccf-196">Quando você concluir o teste do aplicativo, acesse o [portal do Azure](https://portal.azure.com/) e exclua o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-196">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="c8ccf-197">Selecione **Grupos de recursos** e, em seguida, selecione o grupo de recursos criado.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-197">Select **Resource groups**, then select the resource group you created.</span></span>

![Portal do Azure: Grupos de Recursos no menu da barra lateral](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="c8ccf-199">Na página **Grupos de recursos**, selecione **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-199">In the **Resource groups** page, select **Delete**.</span></span>

![Portal do Azure: página Grupos de Recursos](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="c8ccf-201">Insira o nome do grupo de recursos e selecione **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-201">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="c8ccf-202">O aplicativo e todos os outros recursos criados neste tutorial agora foram excluídos do Azure.</span><span class="sxs-lookup"><span data-stu-id="c8ccf-202">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="c8ccf-203">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="c8ccf-203">Next steps</span></span>

* [<span data-ttu-id="c8ccf-204">Implantação contínua no Azure com o Visual Studio e o Git</span><span class="sxs-lookup"><span data-stu-id="c8ccf-204">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="c8ccf-205">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="c8ccf-205">Additonal resources</span></span>

* [<span data-ttu-id="c8ccf-206">Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="c8ccf-206">Azure App Service</span></span>](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="c8ccf-207">Grupo de recursos do Azure</span><span class="sxs-lookup"><span data-stu-id="c8ccf-207">Azure resource groups</span></span>](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="c8ccf-208">Banco de Dados SQL do Azure</span><span class="sxs-lookup"><span data-stu-id="c8ccf-208">Azure SQL Database</span></span>](https://docs.microsoft.com/en-us/azure/sql-database/)

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
ms.openlocfilehash: d0e64c967ff332365981338809a47faf35d499ab
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="ea4cf-103">Publicar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ea4cf-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="ea4cf-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) e [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="ea4cf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="ea4cf-105">Confira [Publicar no Azure do Visual Studio para Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) se você estiver trabalhando em um Mac.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="ea4cf-106">Configurar</span><span class="sxs-lookup"><span data-stu-id="ea4cf-106">Set up</span></span>

* <span data-ttu-id="ea4cf-107">Abra uma [conta do Azure gratuita](https://aka.ms/K5y5yh) se você não tiver uma.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-107">Open a [free Azure account](https://aka.ms/K5y5yh) if you don't have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="ea4cf-108">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="ea4cf-108">Create a web app</span></span>

<span data-ttu-id="ea4cf-109">Na página inicial do Visual Studio, selecione **Arquivo > Novo > Projeto...**</span><span class="sxs-lookup"><span data-stu-id="ea4cf-109">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![Menu Arquivo](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="ea4cf-111">Preencha a caixa de diálogo **Novo Projeto**:</span><span class="sxs-lookup"><span data-stu-id="ea4cf-111">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="ea4cf-112">No painel esquerdo, selecione **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-112">In the left pane, select **.NET Core**.</span></span>
* <span data-ttu-id="ea4cf-113">No painel central, toque em **Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-113">In the center pane, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="ea4cf-114">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-114">Select **OK**.</span></span>

![Caixa de diálogo Novo Projeto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="ea4cf-116">Na caixa de diálogo **Novo Aplicativo Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="ea4cf-116">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="ea4cf-117">Selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-117">Select **Web Application**.</span></span>
* <span data-ttu-id="ea4cf-118">Selecione **Mudar Autenticação**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-118">Select **Change Authentication**.</span></span>

![Caixa de diálogo Novo Projeto](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="ea4cf-120">A caixa de diálogo **Mudar Autenticação** é exibida.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-120">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="ea4cf-121">Selecione **Contas de Usuário Individuais**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-121">Select **Individual User Accounts**.</span></span>
* <span data-ttu-id="ea4cf-122">Selecione **OK** para retornar para o **Novo Aplicativo Web do ASP.NET Core**, em seguida, selecione **OK** novamente.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-122">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Caixa de diálogo Nova autenticação da Web do ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="ea4cf-124">O Visual Studio cria a solução.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-124">Visual Studio creates the solution.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="ea4cf-125">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="ea4cf-125">Run the app</span></span>

* <span data-ttu-id="ea4cf-126">Pressione CTRL+F5 para executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-126">Press CTRL+F5 to run the project.</span></span>
* <span data-ttu-id="ea4cf-127">Teste os links **Sobre** e **Contato**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-127">Test the **About** and **Contact** links.</span></span>

![Aplicativo Web aberto no Microsoft Edge no localhost](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a><span data-ttu-id="ea4cf-129">Registrar um usuário</span><span class="sxs-lookup"><span data-stu-id="ea4cf-129">Register a user</span></span>

* <span data-ttu-id="ea4cf-130">Selecione **Registrar** e registre um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-130">Select **Register** and register a new user.</span></span> <span data-ttu-id="ea4cf-131">Você pode usar um endereço de email fictício.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-131">You can use a fictitious email address.</span></span> <span data-ttu-id="ea4cf-132">Ao enviar, a página exibirá o seguinte erro:</span><span class="sxs-lookup"><span data-stu-id="ea4cf-132">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="ea4cf-133">*"Erro interno do servidor: uma operação de banco de dados falhou ao processar a solicitação. Exceção SQL: não é possível abrir o banco de dados. A aplicação de migrações existentes ao contexto do BD do Aplicativo pode resolver esse problema."*</span><span class="sxs-lookup"><span data-stu-id="ea4cf-133">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>
* <span data-ttu-id="ea4cf-134">Selecione **Aplicar Migrações** e, depois que a página for atualizada, atualize a página.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-134">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Erro interno do servidor: uma operação de banco de dados falhou ao processar a solicitação.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="ea4cf-138">O aplicativo exibe o email usado para registrar o novo usuário e um link **Fazer logout**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-138">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Aplicativo Web aberto no Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="ea4cf-141">Implantar o aplicativo no Azure</span><span class="sxs-lookup"><span data-stu-id="ea4cf-141">Deploy the app to Azure</span></span>

<span data-ttu-id="ea4cf-142">Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e selecione **Publicar...**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-142">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menu contextual aberto com o link Publicar realçado](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="ea4cf-144">Na caixa de diálogo **Publicar**:</span><span class="sxs-lookup"><span data-stu-id="ea4cf-144">In the **Publish** dialog:</span></span>

* <span data-ttu-id="ea4cf-145">Selecione **Serviço de Aplicativo do Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-145">Select **Microsoft Azure App Service**.</span></span>
* <span data-ttu-id="ea4cf-146">Selecione o ícone de engrenagem e, em seguida, **Criar Perfil**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-146">Select the gear icon and then select **Create Profile**.</span></span>
* <span data-ttu-id="ea4cf-147">Selecione **Criar Perfil**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-147">Select **Create Profile**.</span></span>

![Caixa de diálogo Publicar](publish-to-azure-webapp-using-vs/_static/maas1.png)

### <a name="create-azure-resources"></a><span data-ttu-id="ea4cf-149">Criar recursos do Azure</span><span class="sxs-lookup"><span data-stu-id="ea4cf-149">Create Azure resources</span></span>

<span data-ttu-id="ea4cf-150">A caixa de diálogo **Criar Serviço de Aplicativo** será exibida:</span><span class="sxs-lookup"><span data-stu-id="ea4cf-150">The **Create App Service** dialog appears:</span></span>

* <span data-ttu-id="ea4cf-151">Insira sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-151">Enter your subscription.</span></span>
* <span data-ttu-id="ea4cf-152">Os campos de entrada **Nome do Aplicativo**, **Grupo de Recursos** e **Plano do Serviço de Aplicativo** serão populados.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-152">The **App Name**, **Resource Group**, and **App Service Plan** entry fields are populated.</span></span> <span data-ttu-id="ea4cf-153">Você pode manter esses nomes ou alterá-los.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-153">You can keep these names or change them.</span></span>

![Caixa de diálogo Serviço de Aplicativo](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="ea4cf-155">Selecione a guia **Serviços** para criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-155">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="ea4cf-156">Selecione o ícone verde **+** para criar um novo Banco de Dados SQL</span><span class="sxs-lookup"><span data-stu-id="ea4cf-156">Select the green **+** icon to create a new SQL Database</span></span>

![Novo Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="ea4cf-158">Selecione **Novo...** na caixa de diálogo **Configurar Banco de Dados SQL** para criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-158">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Novo servidor e Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="ea4cf-160">A caixa de diálogo **Configurar o SQL Server** é exibida.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-160">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="ea4cf-161">Insira um nome de usuário do administrador e a senha e, em seguida, selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-161">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="ea4cf-162">Você pode manter o **Nome do Servidor** padrão.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-162">You can keep the default **Server Name**.</span></span> 

> [!NOTE]
> <span data-ttu-id="ea4cf-163">“admin” não é permitido como o nome de usuário administrador.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-163">"admin" isn't allowed as the administrator user name.</span></span>

![Caixa de diálogo Configurar o SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="ea4cf-165">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-165">Select **OK**.</span></span>

<span data-ttu-id="ea4cf-166">O Visual Studio retorna para a caixa de diálogo **Criar Serviço de Aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-166">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="ea4cf-167">Selecione **Criar** na caixa de diálogo **Criar Serviço de Aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-167">Select **Create** on the **Create App Service** dialog.</span></span>

![Caixa de diálogo Configurar o Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

<span data-ttu-id="ea4cf-169">O Visual Studio cria o aplicativo Web e o SQL Server no Azure.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-169">Visual Studio creates the Web app and SQL Server on Azure.</span></span> <span data-ttu-id="ea4cf-170">Esta etapa pode levar alguns minutos.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-170">This step can take a few minutes.</span></span> <span data-ttu-id="ea4cf-171">Para obter informações sobre os recursos criados, consulte [Recursos adicionais](#additonal-resources).</span><span class="sxs-lookup"><span data-stu-id="ea4cf-171">For information on the resources created, see [Additonal resources](#additonal-resources).</span></span>

<span data-ttu-id="ea4cf-172">Quando a implantação for concluída, selecione **Configurações**:</span><span class="sxs-lookup"><span data-stu-id="ea4cf-172">When deployment completes, select **Settings**:</span></span>

![Caixa de diálogo Configurar o SQL Server](publish-to-azure-webapp-using-vs/_static/set.png)

<span data-ttu-id="ea4cf-174">Na página **Configurações** da caixa de diálogo **Publicar**:</span><span class="sxs-lookup"><span data-stu-id="ea4cf-174">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="ea4cf-175">Expanda **Bancos de Dados** e marque a opção **Usar esta cadeia de conexão no tempo de execução**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-175">Expand **Databases** and check **Use this connection string at runtime**.</span></span>
  * <span data-ttu-id="ea4cf-176">Expanda **Migrações do Entity Framework** e marque a opção **Aplicar esta migração durante a publicação**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-176">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="ea4cf-177">Selecione **Salvar**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-177">Select **Save**.</span></span> <span data-ttu-id="ea4cf-178">O Visual Studio retorna para a caixa de diálogo **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-178">Visual Studio returns to the **Publish** dialog.</span></span> 

![Caixa de diálogo Publicar: painel Configurações](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="ea4cf-180">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-180">Click **Publish**.</span></span> <span data-ttu-id="ea4cf-181">O Visual Studio publica o aplicativo no Azure.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-181">Visual Studio publishs your app to Azure.</span></span> <span data-ttu-id="ea4cf-182">Quando a implantação for concluída, o aplicativo será aberto em um navegador.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-182">When the depoyment completes, the app is opened in a browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="ea4cf-183">Testar o aplicativo no Azure</span><span class="sxs-lookup"><span data-stu-id="ea4cf-183">Test your app in Azure</span></span>

* <span data-ttu-id="ea4cf-184">Teste os links **Sobre** e **Contato**</span><span class="sxs-lookup"><span data-stu-id="ea4cf-184">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="ea4cf-185">Registrar um novo usuário</span><span class="sxs-lookup"><span data-stu-id="ea4cf-185">Register a new user</span></span>

![Aplicativo Web aberto no Microsoft Edge no Serviço de Aplicativo do Azure](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="ea4cf-187">Atualizar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="ea4cf-187">Update the app</span></span>

* <span data-ttu-id="ea4cf-188">Edite a página do Razor *Pages/About.cshtml* e altere seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-188">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="ea4cf-189">Por exemplo, você pode modificar o parágrafo para indicar “Olá, ASP.NET Core!”: [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="ea4cf-189">For example, you can modify the paragraph to say "Hello ASP.NET Core!": [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="ea4cf-190">Clique com o botão direito do mouse no projeto e selecione **Publicar...** novamente.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-190">Right-click on the project and select **Publish...** again.</span></span>

![Menu contextual aberto com o link Publicar realçado](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="ea4cf-192">Depois que o aplicativo for publicado, verifique se as alterações feitas estão disponíveis no Azure.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-192">After the app is published, verify the changes you made are available on Azure.</span></span>

![Verifique se a tarefa está concluída](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="ea4cf-194">Limpar</span><span class="sxs-lookup"><span data-stu-id="ea4cf-194">Clean up</span></span>

<span data-ttu-id="ea4cf-195">Quando você concluir o teste do aplicativo, acesse o [portal do Azure](https://portal.azure.com/) e exclua o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-195">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="ea4cf-196">Selecione **Grupos de recursos** e, em seguida, selecione o grupo de recursos criado.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-196">Select **Resource groups**, then select the resource group you created.</span></span>

![Portal do Azure: Grupos de Recursos no menu da barra lateral](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="ea4cf-198">Na página **Grupos de recursos**, selecione **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-198">In the **Resource groups** page, select **Delete**.</span></span>

![Portal do Azure: página Grupos de Recursos](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="ea4cf-200">Insira o nome do grupo de recursos e selecione **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-200">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="ea4cf-201">O aplicativo e todos os outros recursos criados neste tutorial agora foram excluídos do Azure.</span><span class="sxs-lookup"><span data-stu-id="ea4cf-201">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="ea4cf-202">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="ea4cf-202">Next steps</span></span>

* [<span data-ttu-id="ea4cf-203">Implantação contínua no Azure com o Visual Studio e o Git</span><span class="sxs-lookup"><span data-stu-id="ea4cf-203">Continuous Deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)

## <a name="additonal-resources"></a><span data-ttu-id="ea4cf-204">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="ea4cf-204">Additonal resources</span></span>

* [<span data-ttu-id="ea4cf-205">Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="ea4cf-205">Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="ea4cf-206">Grupo de recursos do Azure</span><span class="sxs-lookup"><span data-stu-id="ea4cf-206">Azure resource groups</span></span>](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [<span data-ttu-id="ea4cf-207">Banco de Dados SQL do Azure</span><span class="sxs-lookup"><span data-stu-id="ea4cf-207">Azure SQL Database</span></span>](https://docs.microsoft.com/azure/sql-database/)

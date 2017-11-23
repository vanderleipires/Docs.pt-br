---
title: Publicar um aplicativo Web ASP.NET Core no Azure usando o Visual Studio
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 6f697ed4d8876a19cd058533e4f6a5d4f7cdc2fb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="f9b10-103">Publicar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f9b10-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="f9b10-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) e [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="f9b10-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="f9b10-105">Confira [Publicar no Azure do Visual Studio para Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) se você estiver trabalhando em um Mac.</span><span class="sxs-lookup"><span data-stu-id="f9b10-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="f9b10-106">Configurar</span><span class="sxs-lookup"><span data-stu-id="f9b10-106">Set up</span></span>

* <span data-ttu-id="f9b10-107">Abra uma [conta do Azure gratuita](https://aka.ms/K5y5yh) se você não tiver uma.</span><span class="sxs-lookup"><span data-stu-id="f9b10-107">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="f9b10-108">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="f9b10-108">Create a web app</span></span>

<span data-ttu-id="f9b10-109">Na página inicial do Visual Studio, selecione **Arquivo > Novo > Projeto...**</span><span class="sxs-lookup"><span data-stu-id="f9b10-109">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![Menu Arquivo](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="f9b10-111">Preencha a caixa de diálogo **Novo Projeto**:</span><span class="sxs-lookup"><span data-stu-id="f9b10-111">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="f9b10-112">No painel esquerdo, selecione **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-112">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="f9b10-113">No painel central, toque em **Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-113">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="f9b10-114">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-114">Select **OK**.</span></span>

![Caixa de diálogo Novo Projeto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="f9b10-116">Na caixa de diálogo **Novo Aplicativo Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="f9b10-116">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="f9b10-117">Selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-117">Select **Web Application**.</span></span>

* <span data-ttu-id="f9b10-118">Selecione **Mudar Autenticação**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-118">Select **Change Authentication**.</span></span>

![Caixa de diálogo Novo Projeto](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="f9b10-120">A caixa de diálogo **Mudar Autenticação** é exibida.</span><span class="sxs-lookup"><span data-stu-id="f9b10-120">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="f9b10-121">Selecione **Contas de Usuário Individuais**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-121">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="f9b10-122">Selecione **OK** para retornar para o **Novo Aplicativo Web do ASP.NET Core**, em seguida, selecione **OK** novamente.</span><span class="sxs-lookup"><span data-stu-id="f9b10-122">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Caixa de diálogo Nova autenticação da Web do ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="f9b10-124">O Visual Studio cria a solução.</span><span class="sxs-lookup"><span data-stu-id="f9b10-124">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="f9b10-125">Executar o aplicativo localmente</span><span class="sxs-lookup"><span data-stu-id="f9b10-125">Run the app locally</span></span>

* <span data-ttu-id="f9b10-126">Selecione **Depurar** e **Iniciar sem Depurar** para executar o serviço.</span><span class="sxs-lookup"><span data-stu-id="f9b10-126">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="f9b10-127">Clique nos links **Sobre** e **Contato** para verificar se o aplicativo Web funciona.</span><span class="sxs-lookup"><span data-stu-id="f9b10-127">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Aplicativo Web aberto no Microsoft Edge no localhost](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="f9b10-129">Selecione **Registrar** e registre um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="f9b10-129">Select **Register** and register a new user.</span></span> <span data-ttu-id="f9b10-130">Você pode usar um endereço de email fictício.</span><span class="sxs-lookup"><span data-stu-id="f9b10-130">You can use a fictitious email address.</span></span> <span data-ttu-id="f9b10-131">Ao enviar, a página exibirá o seguinte erro:</span><span class="sxs-lookup"><span data-stu-id="f9b10-131">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="f9b10-132">*"Erro interno do servidor: uma operação de banco de dados falhou ao processar a solicitação. Exceção SQL: não é possível abrir o banco de dados. A aplicação de migrações existentes ao contexto do BD do Aplicativo pode resolver esse problema."*</span><span class="sxs-lookup"><span data-stu-id="f9b10-132">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="f9b10-133">Selecione **Aplicar Migrações** e, depois que a página for atualizada, atualize a página.</span><span class="sxs-lookup"><span data-stu-id="f9b10-133">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Erro interno do servidor: uma operação de banco de dados falhou ao processar a solicitação.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="f9b10-137">O aplicativo exibe o email usado para registrar o novo usuário e um link **Fazer logout**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-137">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Aplicativo Web aberto no Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="f9b10-140">Implantar o aplicativo no Azure</span><span class="sxs-lookup"><span data-stu-id="f9b10-140">Deploy the app to Azure</span></span>

<span data-ttu-id="f9b10-141">Feche a página da Web, retorne ao Visual Studio e selecione **Parar Depuração** no menu **Depurar**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-141">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="f9b10-142">Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e selecione **Publicar...**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-142">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menu contextual aberto com o link Publicar realçado](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="f9b10-144">Na caixa de diálogo **Publicar**, selecione **Serviço de Aplicativo do Microsoft Azure** e clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-144">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![Caixa de diálogo Publicar](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="f9b10-146">Nomeie o aplicativo com um nome exclusivo.</span><span class="sxs-lookup"><span data-stu-id="f9b10-146">Name the app a unique name.</span></span> 

* <span data-ttu-id="f9b10-147">Selecione uma assinatura.</span><span class="sxs-lookup"><span data-stu-id="f9b10-147">Select a subscription.</span></span>

* <span data-ttu-id="f9b10-148">Selecione **Novo...** no grupo de recursos e insira um nome para o novo grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="f9b10-148">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="f9b10-149">Selecione **Novo...** no plano do serviço de aplicativo e selecione um local próximo a você.</span><span class="sxs-lookup"><span data-stu-id="f9b10-149">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="f9b10-150">Você pode manter o nome que é gerado por padrão.</span><span class="sxs-lookup"><span data-stu-id="f9b10-150">You can keep the name that is generated by default.</span></span>

![Caixa de diálogo Serviço de Aplicativo](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="f9b10-152">Selecione a guia **Serviços** para criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f9b10-152">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="f9b10-153">Selecione o ícone verde **+** para criar um novo Banco de Dados SQL</span><span class="sxs-lookup"><span data-stu-id="f9b10-153">Select the green **+** icon to create a new SQL Database</span></span>

![Novo Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="f9b10-155">Selecione **Novo...** na caixa de diálogo **Configurar Banco de Dados SQL** para criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f9b10-155">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Novo servidor e Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="f9b10-157">A caixa de diálogo **Configurar o SQL Server** é exibida.</span><span class="sxs-lookup"><span data-stu-id="f9b10-157">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="f9b10-158">Insira um nome de usuário do administrador e a senha e, em seguida, selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-158">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="f9b10-159">Não se esqueça do nome de usuário e da senha criados nesta etapa.</span><span class="sxs-lookup"><span data-stu-id="f9b10-159">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="f9b10-160">Você pode manter o **Nome do Servidor** padrão.</span><span class="sxs-lookup"><span data-stu-id="f9b10-160">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="f9b10-161">Insira nomes para a cadeia de conexão e de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f9b10-161">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="f9b10-162">“admin” não é permitido como o nome de usuário administrador.</span><span class="sxs-lookup"><span data-stu-id="f9b10-162">"admin" is not allowed as the administrator user name.</span></span>

![Caixa de diálogo Configurar o SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="f9b10-164">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-164">Select **OK**.</span></span>

<span data-ttu-id="f9b10-165">O Visual Studio retorna para a caixa de diálogo **Criar Serviço de Aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-165">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="f9b10-166">Selecione **Criar** na caixa de diálogo **Criar Serviço de Aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-166">Select **Create** on the **Create App Service** dialog.</span></span>

![Caixa de diálogo Configurar o Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="f9b10-168">Clique no link **Configurações** na caixa de diálogo **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-168">Click the **Settings** link in the **Publish** dialog.</span></span>

![Caixa de diálogo Publicar: painel Conexão](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="f9b10-170">Na página **Configurações** da caixa de diálogo **Publicar**:</span><span class="sxs-lookup"><span data-stu-id="f9b10-170">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="f9b10-171">Expanda **Bancos de Dados** e marque a opção **Usar esta cadeia de conexão no tempo de execução**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-171">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="f9b10-172">Expanda **Migrações do Entity Framework** e marque a opção **Aplicar esta migração durante a publicação**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-172">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="f9b10-173">Selecione **Salvar**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-173">Select **Save**.</span></span> <span data-ttu-id="f9b10-174">O Visual Studio retorna para a caixa de diálogo **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-174">Visual Studio returns to the **Publish** dialog.</span></span> 

![Caixa de diálogo Publicar: painel Configurações](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="f9b10-176">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-176">Click **Publish**.</span></span> <span data-ttu-id="f9b10-177">O Visual Studio publicará o aplicativo no Azure e iniciará o aplicativo na nuvem no navegador.</span><span class="sxs-lookup"><span data-stu-id="f9b10-177">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="f9b10-178">Testar o aplicativo no Azure</span><span class="sxs-lookup"><span data-stu-id="f9b10-178">Test your app in Azure</span></span>

* <span data-ttu-id="f9b10-179">Teste os links **Sobre** e **Contato**</span><span class="sxs-lookup"><span data-stu-id="f9b10-179">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="f9b10-180">Registrar um novo usuário</span><span class="sxs-lookup"><span data-stu-id="f9b10-180">Register a new user</span></span>

![Aplicativo Web aberto no Microsoft Edge no Serviço de Aplicativo do Azure](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="f9b10-182">Atualizar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="f9b10-182">Update the app</span></span>

* <span data-ttu-id="f9b10-183">Edite a página do Razor *Pages/About.cshtml* e altere seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="f9b10-183">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="f9b10-184">Por exemplo, você pode modificar o parágrafo para dizer "Hello ASP.NET Core!":</span><span class="sxs-lookup"><span data-stu-id="f9b10-184">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="f9b10-185">Clique com o botão direito do mouse no projeto e selecione **Publicar...** novamente.</span><span class="sxs-lookup"><span data-stu-id="f9b10-185">Right-click on the project and select **Publish...** again.</span></span>

![Menu contextual aberto com o link Publicar realçado](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="f9b10-187">Depois que o aplicativo for publicado, verifique se as alterações feitas estão disponíveis no Azure.</span><span class="sxs-lookup"><span data-stu-id="f9b10-187">After the app is published, verify the changes you made are available on Azure.</span></span>

![Verifique se a tarefa está concluída](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="f9b10-189">Limpar</span><span class="sxs-lookup"><span data-stu-id="f9b10-189">Clean up</span></span>

<span data-ttu-id="f9b10-190">Quando você concluir o teste do aplicativo, acesse o [portal do Azure](https://portal.azure.com/) e exclua o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f9b10-190">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="f9b10-191">Selecione **Grupos de recursos** e, em seguida, selecione o grupo de recursos criado.</span><span class="sxs-lookup"><span data-stu-id="f9b10-191">Select **Resource groups**, then select the resource group you created.</span></span>

![Portal do Azure: Grupos de Recursos no menu da barra lateral](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="f9b10-193">Na página **Grupos de recursos**, selecione **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-193">In the **Resource groups** page, select **Delete**.</span></span>

![Portal do Azure: página Grupos de Recursos](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="f9b10-195">Insira o nome do grupo de recursos e selecione **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="f9b10-195">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="f9b10-196">O aplicativo e todos os outros recursos criados neste tutorial agora foram excluídos do Azure.</span><span class="sxs-lookup"><span data-stu-id="f9b10-196">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="f9b10-197">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="f9b10-197">Next steps</span></span>

* [<span data-ttu-id="f9b10-198">Implantação contínua no Azure com o Visual Studio e o Git</span><span class="sxs-lookup"><span data-stu-id="f9b10-198">Continuous Deployment to Azure with Visual Studio and Git</span></span>](../publishing/azure-continuous-deployment.md)

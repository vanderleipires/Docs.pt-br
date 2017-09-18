---
title: Publicar um aplicativo Web ASP.NET Core no Azure usando o Visual Studio
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/01/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 14ce45f0cd15b2de39f722767df076d2c0313787
ms.sourcegitcommit: 6ece943781d8a56784bb6160f14da85210d3fcea
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="b78a0-103">Publicar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b78a0-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="b78a0-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) e [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="b78a0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="b78a0-105">Configurar o ambiente de desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="b78a0-105">Set up the development environment</span></span>

* <span data-ttu-id="b78a0-106">Instale as [Ferramentas do .NET Core + Visual Studio](http://go.microsoft.com/fwlink/?LinkID=798306).</span><span class="sxs-lookup"><span data-stu-id="b78a0-106">Install [.NET Core + Visual Studio tooling](http://go.microsoft.com/fwlink/?LinkID=798306).</span></span>

* <span data-ttu-id="b78a0-107">Confirme sua [conta do Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b78a0-107">Verify your [Azure account](https://portal.azure.com/).</span></span> <span data-ttu-id="b78a0-108">[Abra uma conta gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/) ou [ative os benefícios do assinante do Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="b78a0-108">You can [open a free Azure account](https://azure.microsoft.com/pricing/free-trial/) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="b78a0-109">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="b78a0-109">Create a web app</span></span>

<span data-ttu-id="b78a0-110">Na página inicial do Visual Studio, selecione **Arquivo > Novo > Projeto...**</span><span class="sxs-lookup"><span data-stu-id="b78a0-110">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![Menu Arquivo](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="b78a0-112">Preencha a caixa de diálogo **Novo Projeto**:</span><span class="sxs-lookup"><span data-stu-id="b78a0-112">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="b78a0-113">No painel esquerdo, selecione **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-113">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="b78a0-114">No painel central, toque em **Aplicativo Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-114">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="b78a0-115">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-115">Select **OK**.</span></span>

![Caixa de diálogo Novo Projeto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="b78a0-117">Na caixa de diálogo **Novo Aplicativo Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="b78a0-117">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="b78a0-118">Selecione **Aplicativo Web**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-118">Select **Web Application**.</span></span>

* <span data-ttu-id="b78a0-119">Selecione **Mudar Autenticação**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-119">Select **Change Authentication**.</span></span>

![Caixa de diálogo Novo Projeto](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="b78a0-121">A caixa de diálogo **Mudar Autenticação** é exibida.</span><span class="sxs-lookup"><span data-stu-id="b78a0-121">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="b78a0-122">Selecione **Contas de Usuário Individuais**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-122">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="b78a0-123">Selecione **OK** para retornar para o **Novo Aplicativo Web do ASP.NET Core**, em seguida, selecione **OK** novamente.</span><span class="sxs-lookup"><span data-stu-id="b78a0-123">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Caixa de diálogo Nova autenticação da Web do ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="b78a0-125">O Visual Studio cria a solução.</span><span class="sxs-lookup"><span data-stu-id="b78a0-125">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="b78a0-126">Executar o aplicativo localmente</span><span class="sxs-lookup"><span data-stu-id="b78a0-126">Run the app locally</span></span>

* <span data-ttu-id="b78a0-127">Selecione **Depurar** e **Iniciar sem Depurar** para executar o serviço.</span><span class="sxs-lookup"><span data-stu-id="b78a0-127">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="b78a0-128">Clique nos links **Sobre** e **Contato** para verificar se o aplicativo Web funciona.</span><span class="sxs-lookup"><span data-stu-id="b78a0-128">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Aplicativo Web aberto no Microsoft Edge no localhost](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="b78a0-130">Selecione **Registrar** e registre um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="b78a0-130">Select **Register** and register a new user.</span></span> <span data-ttu-id="b78a0-131">Você pode usar um endereço de email fictício.</span><span class="sxs-lookup"><span data-stu-id="b78a0-131">You can use a fictitious email address.</span></span> <span data-ttu-id="b78a0-132">Ao enviar, a página exibirá o seguinte erro:</span><span class="sxs-lookup"><span data-stu-id="b78a0-132">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="b78a0-133">*"Erro interno do servidor: uma operação de banco de dados falhou ao processar a solicitação. Exceção SQL: não é possível abrir o banco de dados. A aplicação de migrações existentes ao contexto do BD do Aplicativo pode resolver esse problema."*</span><span class="sxs-lookup"><span data-stu-id="b78a0-133">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="b78a0-134">Selecione **Aplicar Migrações** e, depois que a página for atualizada, atualize a página.</span><span class="sxs-lookup"><span data-stu-id="b78a0-134">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Erro interno do servidor: uma operação de banco de dados falhou ao processar a solicitação.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="b78a0-138">O aplicativo exibe o email usado para registrar o novo usuário e um link **Fazer logout**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-138">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Aplicativo Web aberto no Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="b78a0-141">Implantar o aplicativo no Azure</span><span class="sxs-lookup"><span data-stu-id="b78a0-141">Deploy the app to Azure</span></span>

<span data-ttu-id="b78a0-142">Feche a página da Web, retorne ao Visual Studio e selecione **Parar Depuração** no menu **Depurar**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-142">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="b78a0-143">Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e selecione **Publicar...**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-143">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menu contextual aberto com o link Publicar realçado](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="b78a0-145">Na caixa de diálogo **Publicar**, selecione **Serviço de Aplicativo do Microsoft Azure** e clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-145">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![Caixa de diálogo Publicar](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="b78a0-147">Nomeie o aplicativo com um nome exclusivo.</span><span class="sxs-lookup"><span data-stu-id="b78a0-147">Name the app a unique name.</span></span> 

* <span data-ttu-id="b78a0-148">Selecione uma assinatura do MSDN.</span><span class="sxs-lookup"><span data-stu-id="b78a0-148">Select an MSDN subscription.</span></span>

* <span data-ttu-id="b78a0-149">Selecione **Novo...** no grupo de recursos e insira um nome para o novo grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="b78a0-149">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="b78a0-150">Selecione **Novo...** no plano do serviço de aplicativo e selecione um local próximo a você.</span><span class="sxs-lookup"><span data-stu-id="b78a0-150">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="b78a0-151">Você pode manter o nome que é gerado por padrão.</span><span class="sxs-lookup"><span data-stu-id="b78a0-151">You can keep the name that is generated by default.</span></span>

![Caixa de diálogo Serviço de Aplicativo](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="b78a0-153">Selecione a guia **Serviços** para criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b78a0-153">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="b78a0-154">Selecione o ícone verde **+** para criar um novo Banco de Dados SQL</span><span class="sxs-lookup"><span data-stu-id="b78a0-154">Select the green **+** icon to create a new SQL Database</span></span>

![Novo Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="b78a0-156">Selecione **Novo...** na caixa de diálogo **Configurar Banco de Dados SQL** para criar um novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b78a0-156">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Novo servidor e Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="b78a0-158">A caixa de diálogo **Configurar o SQL Server** é exibida.</span><span class="sxs-lookup"><span data-stu-id="b78a0-158">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="b78a0-159">Insira um nome de usuário do administrador e a senha e, em seguida, selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-159">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="b78a0-160">Não se esqueça do nome de usuário e da senha criados nesta etapa.</span><span class="sxs-lookup"><span data-stu-id="b78a0-160">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="b78a0-161">Você pode manter o **Nome do Servidor** padrão.</span><span class="sxs-lookup"><span data-stu-id="b78a0-161">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="b78a0-162">Insira nomes para a cadeia de conexão e de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b78a0-162">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="b78a0-163">“admin” não é permitido como o nome de usuário administrador.</span><span class="sxs-lookup"><span data-stu-id="b78a0-163">"admin" is not allowed as the administrator user name.</span></span>

![Caixa de diálogo Configurar o SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="b78a0-165">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-165">Select **OK**.</span></span>

<span data-ttu-id="b78a0-166">O Visual Studio retorna para a caixa de diálogo **Criar Serviço de Aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-166">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="b78a0-167">Selecione **Criar** na caixa de diálogo **Criar Serviço de Aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-167">Select **Create** on the **Create App Service** dialog.</span></span>

![Caixa de diálogo Configurar o Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="b78a0-169">Clique no link **Configurações** na caixa de diálogo **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-169">Click the **Settings** link in the **Publish** dialog.</span></span>

![Caixa de diálogo Publicar: painel Conexão](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="b78a0-171">Na página **Configurações** da caixa de diálogo **Publicar**:</span><span class="sxs-lookup"><span data-stu-id="b78a0-171">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="b78a0-172">Expanda **Bancos de Dados** e marque a opção **Usar esta cadeia de conexão no tempo de execução**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-172">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="b78a0-173">Expanda **Migrações do Entity Framework** e marque a opção **Aplicar esta migração durante a publicação**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-173">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="b78a0-174">Selecione **Salvar**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-174">Select **Save**.</span></span> <span data-ttu-id="b78a0-175">O Visual Studio retorna para a caixa de diálogo **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-175">Visual Studio returns to the **Publish** dialog.</span></span> 

![Caixa de diálogo Publicar: painel Configurações](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="b78a0-177">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-177">Click **Publish**.</span></span> <span data-ttu-id="b78a0-178">O Visual Studio publicará o aplicativo no Azure e iniciará o aplicativo na nuvem no navegador.</span><span class="sxs-lookup"><span data-stu-id="b78a0-178">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="b78a0-179">Testar o aplicativo no Azure</span><span class="sxs-lookup"><span data-stu-id="b78a0-179">Test your app in Azure</span></span>

* <span data-ttu-id="b78a0-180">Teste os links **Sobre** e **Contato**</span><span class="sxs-lookup"><span data-stu-id="b78a0-180">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="b78a0-181">Registrar um novo usuário</span><span class="sxs-lookup"><span data-stu-id="b78a0-181">Register a new user</span></span>

![Aplicativo Web aberto no Microsoft Edge no Serviço de Aplicativo do Azure](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="b78a0-183">Atualizar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="b78a0-183">Update the app</span></span>

* <span data-ttu-id="b78a0-184">Edite a página do Razor *Pages/About.cshtml* e altere seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="b78a0-184">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="b78a0-185">Por exemplo, você pode modificar o parágrafo para dizer "Hello ASP.NET Core!":</span><span class="sxs-lookup"><span data-stu-id="b78a0-185">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    <span data-ttu-id="b78a0-186">[!code-html[Sobre](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="b78a0-186">[!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]</span></span>

* <span data-ttu-id="b78a0-187">Clique com o botão direito do mouse no projeto e selecione **Publicar...** novamente.</span><span class="sxs-lookup"><span data-stu-id="b78a0-187">Right-click on the project and select **Publish...** again.</span></span>

![Menu contextual aberto com o link Publicar realçado](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="b78a0-189">Depois que o aplicativo for publicado, verifique se as alterações feitas estão disponíveis no Azure.</span><span class="sxs-lookup"><span data-stu-id="b78a0-189">After the app is published, verify the changes you made are available on Azure.</span></span>

![Verifique se a tarefa está concluída](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="b78a0-191">Limpar</span><span class="sxs-lookup"><span data-stu-id="b78a0-191">Clean up</span></span>

<span data-ttu-id="b78a0-192">Quando você concluir o teste do aplicativo, acesse o [portal do Azure](https://portal.azure.com/) e exclua o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b78a0-192">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="b78a0-193">Selecione **Grupos de recursos** e, em seguida, selecione o grupo de recursos criado.</span><span class="sxs-lookup"><span data-stu-id="b78a0-193">Select **Resource groups**, then select the resource group you created.</span></span>

![Portal do Azure: Grupos de Recursos no menu da barra lateral](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="b78a0-195">Na página **Grupos de recursos**, selecione **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-195">In the **Resource groups** page, select **Delete**.</span></span>

![Portal do Azure: página Grupos de Recursos](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="b78a0-197">Insira o nome do grupo de recursos e selecione **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="b78a0-197">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="b78a0-198">O aplicativo e todos os outros recursos criados neste tutorial agora foram excluídos do Azure.</span><span class="sxs-lookup"><span data-stu-id="b78a0-198">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="b78a0-199">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="b78a0-199">Next steps</span></span>

* [<span data-ttu-id="b78a0-200">Introdução ao ASP.NET Core MVC e ao Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b78a0-200">Getting started with ASP.NET Core MVC and Visual Studio</span></span>](first-mvc-app/start-mvc.md)

* [<span data-ttu-id="b78a0-201">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b78a0-201">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="b78a0-202">Conceitos básicos</span><span class="sxs-lookup"><span data-stu-id="b78a0-202">Fundamentals</span></span>](../fundamentals/index.md)

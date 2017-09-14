---
title: Publicar um aplicativo Web ASP.NET Core no Azure usando o Visual Studio
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: c4df5f4551760fbc4ed4b11362d249b24f186e00
ms.sourcegitcommit: 8f5277871eff86134ebf68d3737196cfd4a62c2c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="35d46-103">Publicar um aplicativo Web ASP.NET Core no Serviço de Aplicativo do Azure usando o Visual Studio</span><span class="sxs-lookup"><span data-stu-id="35d46-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="35d46-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Cesar Blum Silveira](https://github.com/cesarbs)</span><span class="sxs-lookup"><span data-stu-id="35d46-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Cesar Blum Silveira](https://github.com/cesarbs)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="35d46-105">Configurar o ambiente de desenvolvimento</span><span class="sxs-lookup"><span data-stu-id="35d46-105">Set up the development environment</span></span>

* <span data-ttu-id="35d46-106">Instale a última versão do [SDK do Azure para Visual Studio](https://www.visualstudio.com/features/azure-tools-vs).</span><span class="sxs-lookup"><span data-stu-id="35d46-106">Install the latest [Azure SDK for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs).</span></span> <span data-ttu-id="35d46-107">O SDK instala o Visual Studio, caso você ainda não tenha feito isso.</span><span class="sxs-lookup"><span data-stu-id="35d46-107">The SDK installs Visual Studio if you don't already have it.</span></span>

> [!NOTE]
> <span data-ttu-id="35d46-108">A instalação do SDK poderá levar mais de 30 minutos, se o computador não tiver muitas das dependências.</span><span class="sxs-lookup"><span data-stu-id="35d46-108">The SDK installation can take more than 30 minutes if your machine doesn't have many of the dependencies.</span></span>

* <span data-ttu-id="35d46-109">Instale o [.NET Core + ferramentas do Visual Studio](http://go.microsoft.com/fwlink/?LinkID=798306)</span><span class="sxs-lookup"><span data-stu-id="35d46-109">Install [.NET Core + Visual Studio tooling](http://go.microsoft.com/fwlink/?LinkID=798306)</span></span>

* <span data-ttu-id="35d46-110">Confirme sua [conta do Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="35d46-110">Verify your [Azure account](https://portal.azure.com/).</span></span> <span data-ttu-id="35d46-111">[Abra uma conta gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/) ou [ative os benefícios do assinante do Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span><span class="sxs-lookup"><span data-stu-id="35d46-111">You can [open a free Azure account](https://azure.microsoft.com/pricing/free-trial/) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="35d46-112">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="35d46-112">Create a web app</span></span>

<span data-ttu-id="35d46-113">No Página Inicial do Visual Studio, toque em **Novo Projeto...**.</span><span class="sxs-lookup"><span data-stu-id="35d46-113">In the Visual Studio Start Page, tap **New Project...**.</span></span>

![Start Page](publish-to-azure-webapp-using-vs/_static/new_project.png)

<span data-ttu-id="35d46-115">Como alternativa, você pode usar os menus para criar um novo projeto.</span><span class="sxs-lookup"><span data-stu-id="35d46-115">Alternatively, you can use the menus to create a new project.</span></span> <span data-ttu-id="35d46-116">Toque em **Arquivo > Novo > Projeto...**.</span><span class="sxs-lookup"><span data-stu-id="35d46-116">Tap **File > New > Project...**.</span></span>

![Menu Arquivo](publish-to-azure-webapp-using-vs/_static/alt_new_project.png)

<span data-ttu-id="35d46-118">Preencha a caixa de diálogo **Novo Projeto**:</span><span class="sxs-lookup"><span data-stu-id="35d46-118">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="35d46-119">No painel esquerdo, toque em **Web**</span><span class="sxs-lookup"><span data-stu-id="35d46-119">In the left pane, tap **Web**</span></span>

* <span data-ttu-id="35d46-120">No painel central, toque em **Aplicativo Web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="35d46-120">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>

* <span data-ttu-id="35d46-121">Toque em **OK**</span><span class="sxs-lookup"><span data-stu-id="35d46-121">Tap **OK**</span></span>

![Caixa de diálogo Novo Projeto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="35d46-123">Na caixa de diálogo **Novo Aplicativo Web ASP.NET Core (.NET Core)**:</span><span class="sxs-lookup"><span data-stu-id="35d46-123">In the **New ASP.NET Core Web Application (.NET Core)** dialog:</span></span>

* <span data-ttu-id="35d46-124">Toque em **Aplicativo Web**</span><span class="sxs-lookup"><span data-stu-id="35d46-124">Tap **Web Application**</span></span>

* <span data-ttu-id="35d46-125">Verifique se a opção **Autenticação** está definida como **Contas de Usuário Individuais**</span><span class="sxs-lookup"><span data-stu-id="35d46-125">Verify **Authentication** is set to **Individual User Accounts**</span></span>

* <span data-ttu-id="35d46-126">Verifique se a opção **Hospedar na nuvem** **não** está marcada</span><span class="sxs-lookup"><span data-stu-id="35d46-126">Verify **Host in the cloud** is **not** checked</span></span>

* <span data-ttu-id="35d46-127">Toque em **OK**</span><span class="sxs-lookup"><span data-stu-id="35d46-127">Tap **OK**</span></span>

![Caixa de diálogo Novo Aplicativo Web ASP.NET Core (.NET Core)](publish-to-azure-webapp-using-vs/_static/noath.png)

## <a name="test-the-app-locally"></a><span data-ttu-id="35d46-129">Testar o aplicativo localmente</span><span class="sxs-lookup"><span data-stu-id="35d46-129">Test the app locally</span></span>

* <span data-ttu-id="35d46-130">Pressione **Ctrl-F5** para executar o aplicativo localmente</span><span class="sxs-lookup"><span data-stu-id="35d46-130">Press **Ctrl-F5** to run the app locally</span></span>

* <span data-ttu-id="35d46-131">Toque nos links **Sobre** e **Contato**.</span><span class="sxs-lookup"><span data-stu-id="35d46-131">Tap the **About** and **Contact** links.</span></span> <span data-ttu-id="35d46-132">Dependendo do tamanho do dispositivo, talvez você precise tocar no ícone de navegação para mostrar os links</span><span class="sxs-lookup"><span data-stu-id="35d46-132">Depending on the size of your device, you might need to tap the navigation icon to show the links</span></span>

![Aplicativo Web aberto no Microsoft Edge no localhost](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="35d46-134">Toque em **Registrar** e registre um novo usuário.</span><span class="sxs-lookup"><span data-stu-id="35d46-134">Tap **Register** and register a new user.</span></span> <span data-ttu-id="35d46-135">Você pode usar um endereço de email fictício.</span><span class="sxs-lookup"><span data-stu-id="35d46-135">You can use a fictitious email address.</span></span> <span data-ttu-id="35d46-136">Ao enviar, você receberá o seguinte erro:</span><span class="sxs-lookup"><span data-stu-id="35d46-136">When you submit, you'll get the following error:</span></span>

![Erro interno do servidor: uma operação de banco de dados falhou ao processar a solicitação.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="35d46-140">Você pode corrigir o problema de duas maneiras diferentes:</span><span class="sxs-lookup"><span data-stu-id="35d46-140">You can fix the problem in two different ways:</span></span>

* <span data-ttu-id="35d46-141">Toque em **Aplicar Migrações** e, depois que a página for atualizada, atualize a página; ou</span><span class="sxs-lookup"><span data-stu-id="35d46-141">Tap **Apply Migrations** and, once the page updates, refresh the page; or</span></span>

* <span data-ttu-id="35d46-142">Execute o seguinte em um prompt de comando no diretório do projeto:</span><span class="sxs-lookup"><span data-stu-id="35d46-142">Run the following from a command prompt in the project's directory:</span></span>

  <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

  ```
  dotnet ef database update
     ```

<span data-ttu-id="35d46-143">O aplicativo exibe o email usado para registrar o novo usuário e um link **Fazer logoff**.</span><span class="sxs-lookup"><span data-stu-id="35d46-143">The app displays the email used to register the new user and a **Log off** link.</span></span>

![Aplicativo Web aberto no Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="35d46-146">Implantar o aplicativo no Azure</span><span class="sxs-lookup"><span data-stu-id="35d46-146">Deploy the app to Azure</span></span>

<span data-ttu-id="35d46-147">Confirme se o aplicativo publicado para implantação não está em execução.</span><span class="sxs-lookup"><span data-stu-id="35d46-147">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="35d46-148">Os arquivos da pasta *publish* são bloqueados quando o aplicativo está em execução.</span><span class="sxs-lookup"><span data-stu-id="35d46-148">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="35d46-149">A implantação não pode ocorrer porque os arquivos bloqueados não podem ser copiados.</span><span class="sxs-lookup"><span data-stu-id="35d46-149">Deployment can't occur because locked files can't be copied.</span></span>

<span data-ttu-id="35d46-150">Clique com o botão direito do mouse no projeto no Gerenciador de Soluções e selecione **Publicar...**.</span><span class="sxs-lookup"><span data-stu-id="35d46-150">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Menu contextual aberto com o link Publicar realçado](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="35d46-152">Na caixa de diálogo **Publicar**, toque em **Serviço de Aplicativo do Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="35d46-152">In the **Publish** dialog, tap **Microsoft Azure App Service**.</span></span>

![Caixa de diálogo Publicar](publish-to-azure-webapp-using-vs/_static/maas1.png)

<span data-ttu-id="35d46-154">Toque em **Novo...** para criar um novo grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="35d46-154">Tap **New...** to create a new resource group.</span></span> <span data-ttu-id="35d46-155">A criação de um novo grupo de recursos facilitará a exclusão de todos os recursos do Azure criados neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="35d46-155">Creating a new resource group will make it easier to delete all the Azure resources you create in this tutorial.</span></span>

![Caixa de diálogo Serviço de Aplicativo](publish-to-azure-webapp-using-vs/_static/newrg1.png)

<span data-ttu-id="35d46-157">Crie um novo grupo de recursos e um plano do serviço de aplicativo:</span><span class="sxs-lookup"><span data-stu-id="35d46-157">Create a new resource group and app service plan:</span></span>

* <span data-ttu-id="35d46-158">Toque em **Novo...** no grupo de recursos e insira um nome para o novo grupo de recursos</span><span class="sxs-lookup"><span data-stu-id="35d46-158">Tap **New...** for the resource group and enter a name for the new resource group</span></span>

* <span data-ttu-id="35d46-159">Toque em **Novo...** no plano do serviço de aplicativo e selecione um local próximo a você.</span><span class="sxs-lookup"><span data-stu-id="35d46-159">Tap **New...** for the  app service plan and select a location near you.</span></span> <span data-ttu-id="35d46-160">Você pode manter o nome padrão gerado</span><span class="sxs-lookup"><span data-stu-id="35d46-160">You can keep the default generated name</span></span>

* <span data-ttu-id="35d46-161">Toque em **Explorar serviços adicionais do Azure** para criar um novo banco de dados</span><span class="sxs-lookup"><span data-stu-id="35d46-161">Tap **Explore additional Azure services** to create a new database</span></span>

![Caixa de diálogo Novo Grupo de Recursos: painel Hospedagem](publish-to-azure-webapp-using-vs/_static/cas.png)

* <span data-ttu-id="35d46-163">Toque no ícone verde **+** para criar um novo Banco de Dados SQL</span><span class="sxs-lookup"><span data-stu-id="35d46-163">Tap the green **+** icon to create a new SQL Database</span></span>

![Caixa de diálogo Novo Grupo de Recursos: painel Serviços](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="35d46-165">Toque em **Novo...** na caixa de diálogo **Configurar Banco de Dados SQL** para criar um novo servidor de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="35d46-165">Tap **New...** on the **Configure SQL Database** dialog to create a new database server.</span></span>

![Caixa de diálogo Configurar o Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

* <span data-ttu-id="35d46-167">Insira um nome de usuário administrador e a senha e, em seguida, toque em **OK**.</span><span class="sxs-lookup"><span data-stu-id="35d46-167">Enter an administrator user name and password, and then tap **OK**.</span></span> <span data-ttu-id="35d46-168">Não se esqueça do nome de usuário e da senha criados nesta etapa.</span><span class="sxs-lookup"><span data-stu-id="35d46-168">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="35d46-169">Você pode manter o **Nome do Servidor** padrão</span><span class="sxs-lookup"><span data-stu-id="35d46-169">You can keep the default **Server Name**</span></span>

![Caixa de diálogo Configurar o SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

> [!NOTE]
> <span data-ttu-id="35d46-171">“admin” não é permitido como o nome de usuário administrador.</span><span class="sxs-lookup"><span data-stu-id="35d46-171">"admin" is not allowed as the administrator user name.</span></span>

* <span data-ttu-id="35d46-172">Toque em **OK** na caixa de diálogo **Configurar Banco de Dados SQL**</span><span class="sxs-lookup"><span data-stu-id="35d46-172">Tap **OK** on the  **Configure SQL Database** dialog</span></span>

![Caixa de diálogo Configurar o Banco de Dados SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="35d46-174">Toque em **Criar** na caixa de diálogo **Criar Serviço de Aplicativo**</span><span class="sxs-lookup"><span data-stu-id="35d46-174">Tap **Create** on the **Create App Service** dialog</span></span>

![Caixa de diálogo Criar Serviço de Aplicativo](publish-to-azure-webapp-using-vs/_static/create_as.png)

* <span data-ttu-id="35d46-176">Toque em **Avançar** na caixa de diálogo **Publicar**</span><span class="sxs-lookup"><span data-stu-id="35d46-176">Tap **Next** in the **Publish** dialog</span></span>

![Caixa de diálogo Publicar: painel Conexão](publish-to-azure-webapp-using-vs/_static/pubc.png)

* <span data-ttu-id="35d46-178">No estágio **Configurações** da caixa de diálogo **Publicar**:</span><span class="sxs-lookup"><span data-stu-id="35d46-178">On the **Settings** stage of the **Publish** dialog:</span></span>

  * <span data-ttu-id="35d46-179">Expanda **Bancos de Dados** e marque a opção **Usar esta cadeia de conexão em tempo de execução**</span><span class="sxs-lookup"><span data-stu-id="35d46-179">Expand **Databases** and check **Use this connection string at runtime**</span></span>

  * <span data-ttu-id="35d46-180">Expanda **Migrações do Entity Framework** e marque a opção **Aplicar esta migração durante a publicação**</span><span class="sxs-lookup"><span data-stu-id="35d46-180">Expand **Entity Framework Migrations** and check **Apply this migration on publish**</span></span>

* <span data-ttu-id="35d46-181">Toque em **Publicar** e aguarde até que o Visual Studio conclua a publicação do aplicativo</span><span class="sxs-lookup"><span data-stu-id="35d46-181">Tap **Publish** and wait until Visual Studio finishes publishing your app</span></span>

![Caixa de diálogo Publicar: painel Configurações](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="35d46-183">O Visual Studio publicará o aplicativo no Azure e iniciará o aplicativo na nuvem no navegador.</span><span class="sxs-lookup"><span data-stu-id="35d46-183">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="35d46-184">Testar o aplicativo no Azure</span><span class="sxs-lookup"><span data-stu-id="35d46-184">Test your app in Azure</span></span>

* <span data-ttu-id="35d46-185">Teste os links **Sobre** e **Contato**</span><span class="sxs-lookup"><span data-stu-id="35d46-185">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="35d46-186">Registrar um novo usuário</span><span class="sxs-lookup"><span data-stu-id="35d46-186">Register a new user</span></span>

![Aplicativo Web aberto no Microsoft Edge no Serviço de Aplicativo do Azure](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="update-the-app"></a><span data-ttu-id="35d46-188">Atualizar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="35d46-188">Update the app</span></span>

* <span data-ttu-id="35d46-189">Edite o arquivo de exibição `Views/Home/About.cshtml` do Razor e altere seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="35d46-189">Edit the `Views/Home/About.cshtml` Razor view file and change its contents.</span></span> <span data-ttu-id="35d46-190">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="35d46-190">For example:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [7]}} -->

```html
@{
       ViewData["Title"] = "About";
   }
   <h2>@ViewData["Title"].</h2>
   <h3>@ViewData["Message"]</h3>

   <p>My updated about page.</p>
   ```

* <span data-ttu-id="35d46-191">Clique com o botão direito do mouse no projeto e toque em **Publicar...**  novamente</span><span class="sxs-lookup"><span data-stu-id="35d46-191">Right-click on the project and tap **Publish...** again</span></span>

![Menu contextual aberto com o link Publicar realçado](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="35d46-193">Depois que o aplicativo for publicado, verifique se as alterações feitas estão disponíveis no Azure</span><span class="sxs-lookup"><span data-stu-id="35d46-193">After the app is published, verify the changes you made are available on Azure</span></span>

### <a name="clean-up"></a><span data-ttu-id="35d46-194">Limpar</span><span class="sxs-lookup"><span data-stu-id="35d46-194">Clean up</span></span>

<span data-ttu-id="35d46-195">Quando você concluir o teste do aplicativo, acesse o [portal do Azure](https://portal.azure.com/) e exclua o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="35d46-195">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="35d46-196">Selecione **Grupos de recursos** e, em seguida, toque no grupo de recursos criado</span><span class="sxs-lookup"><span data-stu-id="35d46-196">Select **Resource groups**, then tap the resource group you created</span></span>

![Portal do Azure: Grupos de Recursos no menu da barra lateral](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="35d46-198">Na folha **Grupo de recursos**, toque em **Excluir**</span><span class="sxs-lookup"><span data-stu-id="35d46-198">In the **Resource group** blade, tap **Delete**</span></span>

![Portal do Azure: folha Grupos de Recursos](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="35d46-200">Insira o nome do grupo de recursos e toque em **Excluir**.</span><span class="sxs-lookup"><span data-stu-id="35d46-200">Enter the name of the resource group and tap **Delete**.</span></span> <span data-ttu-id="35d46-201">O aplicativo e todos os outros recursos criados neste tutorial agora foram excluídos do Azure</span><span class="sxs-lookup"><span data-stu-id="35d46-201">Your app and all other resources created in this tutorial are now deleted from Azure</span></span>

### <a name="next-steps"></a><span data-ttu-id="35d46-202">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="35d46-202">Next steps</span></span>

* [<span data-ttu-id="35d46-203">Introdução ao ASP.NET Core MVC e ao Visual Studio</span><span class="sxs-lookup"><span data-stu-id="35d46-203">Getting started with ASP.NET Core MVC and Visual Studio</span></span>](first-mvc-app/start-mvc.md)

* [<span data-ttu-id="35d46-204">Introdução ao ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="35d46-204">Introduction to ASP.NET Core</span></span>](../index.md)

* [<span data-ttu-id="35d46-205">Conceitos básicos</span><span class="sxs-lookup"><span data-stu-id="35d46-205">Fundamentals</span></span>](../fundamentals/index.md)

---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Publicar o site MVC banco de dados primeiro no Azure | Microsoft Docs
author: tfitzmac
description: Usando o MVC, Entity Framework e estrutura do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Este tutorial série...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 839bbceba6f0e098303facd40dbb1496bd449ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869953"
---
<a name="publish-mvc-database-first-site-to-azure"></a><span data-ttu-id="f1530-104">Publicar o site MVC banco de dados primeiro no Azure</span><span class="sxs-lookup"><span data-stu-id="f1530-104">Publish MVC Database First site to Azure</span></span>
====================
<span data-ttu-id="f1530-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f1530-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f1530-106">Usando o MVC, Entity Framework e estrutura do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="f1530-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="f1530-107">Esta série de tutorial mostra como automaticamente gerar um código que permite aos usuários exibir, editar, criar e excluir dados que residem em uma tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="f1530-108">O código gerado corresponde às colunas na tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="f1530-109">Esta parte da série se concentra em publicar o aplicativo web e o banco de dados no Azure.</span><span class="sxs-lookup"><span data-stu-id="f1530-109">This part of the series focuses on publishing the web app and database to Azure.</span></span> <span data-ttu-id="f1530-110">Você pode ler este tópico para saber mais sobre como publicar um aplicativo web e o banco de dados, mas, na verdade, execute as etapas que você deve iniciar no início do tutorial.</span><span class="sxs-lookup"><span data-stu-id="f1530-110">You can read this topic to learn about publishing a web app and database, but to actually perform the steps you must start at the beginning of the tutorial.</span></span> <span data-ttu-id="f1530-111">Consulte [Introdução](setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="f1530-111">See [Getting Started](setting-up-database.md).</span></span>


## <a name="deploy-your-web-app-on-azure"></a><span data-ttu-id="f1530-112">Implantar o aplicativo web no Azure</span><span class="sxs-lookup"><span data-stu-id="f1530-112">Deploy your web app on Azure</span></span>

<span data-ttu-id="f1530-113">Você precisa de uma conta do Azure para concluir este tutorial:</span><span class="sxs-lookup"><span data-stu-id="f1530-113">You need an Azure account to complete this tutorial:</span></span>

- <span data-ttu-id="f1530-114">Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -você obtém créditos você pode usar para experimentar os serviços do Azure pagos e mesmo depois que eles são usados até que você pode manter a conta e livre de usar os serviços do Azure.</span><span class="sxs-lookup"><span data-stu-id="f1530-114">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="f1530-115">Você pode [ativar os benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -assinatura do MSDN fornece créditos de cada mês em que você pode usar para os serviços do Azure pagos.</span><span class="sxs-lookup"><span data-stu-id="f1530-115">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

<span data-ttu-id="f1530-116">Para publicar seu aplicativo web, clique com o botão direito e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="f1530-116">To publish your web app, right-click the project and select **Publish**.</span></span>

![publicar o site](publish-to-azure/_static/image1.png)

<span data-ttu-id="f1530-118">Selecione os sites do Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f1530-118">Select Microsoft Azure Websites.</span></span>

![Selecionar o Azure](publish-to-azure/_static/image2.png)

<span data-ttu-id="f1530-120">Se você não tiver entrado Azure, forneça suas credenciais de conta do Azure.</span><span class="sxs-lookup"><span data-stu-id="f1530-120">If you are not signed in to Azure, provide your Azure account credentials.</span></span> <span data-ttu-id="f1530-121">Em seguida, selecione Novo para criar um novo aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="f1530-121">Then, select New to create a new web app.</span></span>

![novo site](publish-to-azure/_static/image3.png)

<span data-ttu-id="f1530-123">Crie um nome exclusivo para seu aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="f1530-123">Create a unique name for your web app.</span></span> <span data-ttu-id="f1530-124">Você saberá que o nome é exclusivo se houver uma marca de seleção verde à direita do campo nome.</span><span class="sxs-lookup"><span data-stu-id="f1530-124">You will know the name is unique if you see a green check mark to the right of the name field.</span></span> <span data-ttu-id="f1530-125">Selecione uma região para seu aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="f1530-125">Select a region for your web app.</span></span> <span data-ttu-id="f1530-126">Selecione **criar novo servidor** para o banco de dados e forneça um nome de usuário e senha para o novo servidor de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-126">Select **Create new server** for the database, and provide a user name and password for this new database server.</span></span> <span data-ttu-id="f1530-127">Quando terminar, clique em **criar**.</span><span class="sxs-lookup"><span data-stu-id="f1530-127">When finished, click **Create**.</span></span>

![Criar site](publish-to-azure/_static/image4.png)

<span data-ttu-id="f1530-129">Os valores de conexão estão agora definidos.</span><span class="sxs-lookup"><span data-stu-id="f1530-129">Your connection values are now all set.</span></span> <span data-ttu-id="f1530-130">Você pode deixar esses valores inalterados.</span><span class="sxs-lookup"><span data-stu-id="f1530-130">You can leave these values unchanged.</span></span>

![conexão](publish-to-azure/_static/image5.png)

<span data-ttu-id="f1530-132">Clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="f1530-132">Click **Next**.</span></span>

<span data-ttu-id="f1530-133">Para configurações, observe que dois banco de dados conexões são especificadas - ApplicationDBContext e ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="f1530-133">For Settings, notice that two database connections are specified - ApplicationDBContext and ContosoUniversityDataEntities.</span></span> <span data-ttu-id="f1530-134">ApplicationDBContext é a conexão para as tabelas de conta de usuário.</span><span class="sxs-lookup"><span data-stu-id="f1530-134">ApplicationDBContext is the connection for user account tables.</span></span> <span data-ttu-id="f1530-135">Esses valores mostram apenas as cadeias de caracteres de conexão para bancos de dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-135">These values only show the connection strings for the databases.</span></span> <span data-ttu-id="f1530-136">Isso não significa que esses bancos de dados serão publicados quando você publica seu site.</span><span class="sxs-lookup"><span data-stu-id="f1530-136">It does not mean that these databases will be published when you publish your site.</span></span> <span data-ttu-id="f1530-137">Depois de publicar o aplicativo web, você publicará seu projeto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-137">You will publish your database project after you have finished publishing the web app.</span></span>

![](publish-to-azure/_static/image6.png)

<span data-ttu-id="f1530-138">As reticências (...) ao lado de conexão de banco de dados mostra os detalhes da cadeia de caracteres de conexão.</span><span class="sxs-lookup"><span data-stu-id="f1530-138">The ellipsis (...) next to the database connection shows you the details of the connection string.</span></span> <span data-ttu-id="f1530-139">Clique no botão de reticências ao lado de ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="f1530-139">Click the ellipsis next to ContosoUniversityDataEntities.</span></span>

![](publish-to-azure/_static/image7.png)

<span data-ttu-id="f1530-140">Observe o nome do servidor de banco de dados e o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-140">Note the name of the database server and the database.</span></span> <span data-ttu-id="f1530-141">O nome do servidor é gerado aleatoriamente.</span><span class="sxs-lookup"><span data-stu-id="f1530-141">The server name is randomly generated.</span></span> <span data-ttu-id="f1530-142">O nome do banco de dados é simple o nome do seu site com  **\_db** anexado.</span><span class="sxs-lookup"><span data-stu-id="f1530-142">The database name is simple the name of your site with **\_db** appended.</span></span> <span data-ttu-id="f1530-143">Você precisará mais tarde os dois nomes quando você publica seu banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-143">You will need both names later when you publish your database.</span></span>

<span data-ttu-id="f1530-144">Clique em **Okey** para fechar a janela de cadeia de caracteres de conexão de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-144">Click **OK** to close the database connection string window.</span></span>

<span data-ttu-id="f1530-145">Na janela de publicar Web, clique em **próximo** para exibir a visualização.</span><span class="sxs-lookup"><span data-stu-id="f1530-145">In the Publish Web window, click **Next** to see the preview.</span></span>

![](publish-to-azure/_static/image8.png)

<span data-ttu-id="f1530-146">Você pode clicar em Iniciar a visualização para ver uma lista de arquivos para publicar.</span><span class="sxs-lookup"><span data-stu-id="f1530-146">You can click Start Preview to see a list of the files to publish.</span></span> <span data-ttu-id="f1530-147">Como esta é na primeira vez que você publicou este site, a lista é todos os arquivos relevantes no projeto.</span><span class="sxs-lookup"><span data-stu-id="f1530-147">Since this is the first time you have published this site, the list is every relevant file in the project.</span></span>

<span data-ttu-id="f1530-148">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="f1530-148">Click **Publish**.</span></span>

<span data-ttu-id="f1530-149">O painel de saída exibirá o resultado da publicação.</span><span class="sxs-lookup"><span data-stu-id="f1530-149">The Output pane will display the result of your publication.</span></span>

![](publish-to-azure/_static/image9.png)

<span data-ttu-id="f1530-150">Após a publicação, o site é immedialely iniciado em um navegador da web.</span><span class="sxs-lookup"><span data-stu-id="f1530-150">After publication, the site is immedialely launched in a web browser.</span></span> <span data-ttu-id="f1530-151">O site foi implantado e você pode registrar um novo usuário para o site. No entanto, as tabelas no projeto ContosoUniversityData ainda não foram publicadas.</span><span class="sxs-lookup"><span data-stu-id="f1530-151">Your site has been deployed and you can register a new user to the site; however, your tables in the ContosoUniversityData project have not yet been published.</span></span> <span data-ttu-id="f1530-152">Se você clicar na lista de link de alunos, você receberá um erro.</span><span class="sxs-lookup"><span data-stu-id="f1530-152">If you click on the List of students link you will receive an error.</span></span>

## <a name="publish-database-to-sql-azure"></a><span data-ttu-id="f1530-153">Publicar o banco de dados para o SQL Azure</span><span class="sxs-lookup"><span data-stu-id="f1530-153">Publish database to SQL Azure</span></span>

<span data-ttu-id="f1530-154">Antes de publicar seu banco de dados, você deve verificar se o que computador local pode se conectar ao servidor de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-154">Before publishing your database, you must make sure your local computer can connect to the database server.</span></span> <span data-ttu-id="f1530-155">O firewall para o servidor de banco de dados restringe quais computadores podem se conectar ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-155">The firewall for your database server restricts which machines can connect to the database.</span></span> <span data-ttu-id="f1530-156">Você precisa adicionar o endereço IP do seu computador para os endereços IP permitidos para o firewall.</span><span class="sxs-lookup"><span data-stu-id="f1530-156">You need to add the IP address of your computer to the allowed IP addresses for the firewall.</span></span>

<span data-ttu-id="f1530-157">Faça logon em sua conta do Azure por meio do portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="f1530-157">Login to your Azure account through the Azure portal.</span></span>

<span data-ttu-id="f1530-158">Selecione o novo banco de dados e selecione **gerenciar**.</span><span class="sxs-lookup"><span data-stu-id="f1530-158">Select your new database and select **Manage**.</span></span>

![Gerenciar](publish-to-azure/_static/image10.png)

<span data-ttu-id="f1530-160">Você deve configurar o servidor de banco de dados para permitir conexões do seu computador.</span><span class="sxs-lookup"><span data-stu-id="f1530-160">You must configure your database server to allow connections from your computer.</span></span> <span data-ttu-id="f1530-161">Quando você seleciona a gerenciar, você será solicitado a adicionar o endereço IP atual, conforme permitido para o servidor de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-161">When you select Manage, you are asked to add the current IP address as permitted to the database server.</span></span> <span data-ttu-id="f1530-162">Selecione Sim.</span><span class="sxs-lookup"><span data-stu-id="f1530-162">Select Yes.</span></span>

![Adicionar endereço ip](publish-to-azure/_static/image11.png)

<span data-ttu-id="f1530-164">Há uma chance de que o endereço IP que você adicionou na etapa anterior não é o endereço IP só que é necessário configurar para conexões.</span><span class="sxs-lookup"><span data-stu-id="f1530-164">There is a chance that the IP address you added in the previous step is not the only IP address you need to configure for connections.</span></span> <span data-ttu-id="f1530-165">Você pode tentar fazer logon no banco de dados para ver se as conexões foram corretamente configuradas.</span><span class="sxs-lookup"><span data-stu-id="f1530-165">You can attempt to login to the database to see if the connections have been properly set up.</span></span> <span data-ttu-id="f1530-166">Forneça o usuário e a senha que você criou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="f1530-166">Provide the user and password you created earlier.</span></span>

![Logon](publish-to-azure/_static/image12.png)

<span data-ttu-id="f1530-168">Se você receber uma mensagem de erro, você precisa adicionar outro endereço IP.</span><span class="sxs-lookup"><span data-stu-id="f1530-168">If you receive an error message, you need to add another IP address.</span></span> <span data-ttu-id="f1530-169">Clique na mensagem de erro para ver mais detalhes sobre o erro.</span><span class="sxs-lookup"><span data-stu-id="f1530-169">Click the error message to see more details about error.</span></span> <span data-ttu-id="f1530-170">Os detalhes, você verá o endereço IP que você precisa adicionar.</span><span class="sxs-lookup"><span data-stu-id="f1530-170">In the details you will see the IP address that you need to add.</span></span> <span data-ttu-id="f1530-171">Observe que esse endereço IP.</span><span class="sxs-lookup"><span data-stu-id="f1530-171">Note this IP address.</span></span>

![não permitido](publish-to-azure/_static/image13.png)

<span data-ttu-id="f1530-173">Feche esta janela de logon e retornar ao portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="f1530-173">Close this login window, and return to the Azure portal.</span></span>

<span data-ttu-id="f1530-174">Navegue até o painel para seu banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-174">Navigate to the Dashboard for your database.</span></span> <span data-ttu-id="f1530-175">Clique em **gerenciar endereços IP permitido**.</span><span class="sxs-lookup"><span data-stu-id="f1530-175">Click **Manage allowed IP addresses**.</span></span>

![gerenciar endereços ip](publish-to-azure/_static/image14.png)

<span data-ttu-id="f1530-177">Agora você deve adicionar o endereço IP da mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="f1530-177">You must now add the IP address from the error message.</span></span> <span data-ttu-id="f1530-178">Altere o intervalo de endereços IP permitidos para incluir uma na mensagem de erro, ou adicionar esse endereço IP como uma entrada separada.</span><span class="sxs-lookup"><span data-stu-id="f1530-178">Either change the range of allowed IP addresses to include the one from the error message, or add that IP address as a separate entry.</span></span>

![Adicionar novo endereço](publish-to-azure/_static/image15.png)

<span data-ttu-id="f1530-180">Salve a alteração de endereços IP permitidos.</span><span class="sxs-lookup"><span data-stu-id="f1530-180">Save the change to allowed IP addresses.</span></span>

<span data-ttu-id="f1530-181">Clique em gerenciar e tente fazer logon novamente para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-181">Click Manage, and try logging in again to the database.</span></span> <span data-ttu-id="f1530-182">Talvez seja necessário aguardar alguns minutos antes dos endereços IP permitidos estão configurados corretamente para o firewall.</span><span class="sxs-lookup"><span data-stu-id="f1530-182">You may need to wait a few minutes before the allowed IP addresses are correctly configured for the firewall.</span></span> <span data-ttu-id="f1530-183">Quando você pode fazer logon com êxito no banco de dados, você configurou sua conexão ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-183">When you can successfully log in the database, you have finished setting up your connection to the database.</span></span>

<span data-ttu-id="f1530-184">Você pode deixar esta janela de gerenciamento aberta porque você verificará o resultado da implantação do banco de dados em breve.</span><span class="sxs-lookup"><span data-stu-id="f1530-184">You can leave this management window open because you will check the result of your database deployment shortly.</span></span>

<span data-ttu-id="f1530-185">Retornar ao seu projeto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-185">Return to your database project.</span></span> <span data-ttu-id="f1530-186">Clique com o botão direito e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="f1530-186">Right-click the project and select **Publish**.</span></span>

![publicar o banco de dados](publish-to-azure/_static/image16.png)

<span data-ttu-id="f1530-188">Na janela do banco de dados de publicação, selecione **editar**.</span><span class="sxs-lookup"><span data-stu-id="f1530-188">In the Publish Database window, select **Edit**.</span></span>

![editar](publish-to-azure/_static/image17.png)

<span data-ttu-id="f1530-190">Forneça o nome do servidor de banco de dados e suas credenciais de autenticação para o servidor.</span><span class="sxs-lookup"><span data-stu-id="f1530-190">Provide the name of the database server and your authentication credentials for the server.</span></span> <span data-ttu-id="f1530-191">Depois de fornecer as credenciais, selecione o banco de dados que você criou na lista de bancos de dados disponíveis.</span><span class="sxs-lookup"><span data-stu-id="f1530-191">After providing the credentials, select the database you created from the list of available databases.</span></span> <span data-ttu-id="f1530-192">Por padrão, o Visual Studio define o nome do campo de banco de dados para o nome do projeto que pode não ser o mesmo que o banco de dados que você criou.</span><span class="sxs-lookup"><span data-stu-id="f1530-192">By default, Visual Studio sets the name of the database field to the name of your project which might not be the same as the database you created.</span></span>

![](publish-to-azure/_static/image18.png)

<span data-ttu-id="f1530-193">Clique em OK.</span><span class="sxs-lookup"><span data-stu-id="f1530-193">Click OK.</span></span>

<span data-ttu-id="f1530-194">Você provavelmente desejará salvar esse perfil, você pode publicar as atualizações no futuro sem inserir novamente todas as informações de conexão.</span><span class="sxs-lookup"><span data-stu-id="f1530-194">You will probably want to save this profile so you can publish updates in the future without re-entering all of the connection information.</span></span> <span data-ttu-id="f1530-195">Selecione **Criar Perfil**.</span><span class="sxs-lookup"><span data-stu-id="f1530-195">Select **Create Profile**.</span></span>

![Salvar perfil](publish-to-azure/_static/image19.png)

<span data-ttu-id="f1530-197">Ele criará um arquivo no seu projeto chamado **ContosoUniversityData.publish.xml**.</span><span class="sxs-lookup"><span data-stu-id="f1530-197">It will create a file in your project named **ContosoUniversityData.publish.xml**.</span></span> <span data-ttu-id="f1530-198">Na próxima vez em que você deseja publicar o banco de dados no Azure, basta carrega perfil.</span><span class="sxs-lookup"><span data-stu-id="f1530-198">The next time you want to publish the database to Azure, simply load that profile.</span></span>

<span data-ttu-id="f1530-199">Agora, clique em **publicar** para criar o banco de dados no Azure.</span><span class="sxs-lookup"><span data-stu-id="f1530-199">Now, click **Publish** to create the database on Azure.</span></span>

![publicar](publish-to-azure/_static/image20.png)

<span data-ttu-id="f1530-201">Após ser executado por um tempo, a publicação de resultados é exibido.</span><span class="sxs-lookup"><span data-stu-id="f1530-201">After running for a while, the publishing results are displayed.</span></span>

![resultados](publish-to-azure/_static/image21.png)

<span data-ttu-id="f1530-203">Agora, você pode voltar ao portal de gerenciamento para seu banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-203">Now, you can go back to the management portal for your database.</span></span> <span data-ttu-id="f1530-204">Atualizar a exibição de design e observe que as 3 tabelas com dados previamente preenchidos foram implantadas.</span><span class="sxs-lookup"><span data-stu-id="f1530-204">Refresh the design view, and notice the 3 tables with pre-filled data have been deployed.</span></span>

![novas tabelas](publish-to-azure/_static/image22.png)

<span data-ttu-id="f1530-206">Agora você está pronto para testar o aplicativo web que é implantado no Azure.</span><span class="sxs-lookup"><span data-stu-id="f1530-206">Now you are ready to test the web app that is deployed to Azure.</span></span> <span data-ttu-id="f1530-207">Navegue até o aplicativo web no Azure (como http://contosositeexample.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="f1530-207">Navigate to the web app on Azure (such as http://contosositeexample.azurewebsites.net/).</span></span> <span data-ttu-id="f1530-208">Clique no link para a lista de alunos e você deverá ver a exibição do índice para estudantes.</span><span class="sxs-lookup"><span data-stu-id="f1530-208">Click the link for List of students and you should see the index view for students.</span></span>

![Modo de exibição](publish-to-azure/_static/image23.png)

<span data-ttu-id="f1530-210">Ocasionalmente, o banco de dados e a conexão precisam de algum tempo para ser configurado corretamente.</span><span class="sxs-lookup"><span data-stu-id="f1530-210">Occasionally, the database and connection need a little time to be properly configured.</span></span> <span data-ttu-id="f1530-211">Se você receber um erro, aguarde alguns minutos e tente novamente.</span><span class="sxs-lookup"><span data-stu-id="f1530-211">If you receive an error, wait a few minutes and then try again.</span></span>

## <a name="conclusion"></a><span data-ttu-id="f1530-212">Conclusão</span><span class="sxs-lookup"><span data-stu-id="f1530-212">Conclusion</span></span>

<span data-ttu-id="f1530-213">Esta série fornecido um exemplo simples de como gerar código a partir de um banco de dados que permite aos usuários editar, atualizar, criar e excluir dados.</span><span class="sxs-lookup"><span data-stu-id="f1530-213">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="f1530-214">Ele usado ASP.NET MVC 5, o Entity Framework e o ASP.NET Scaffolding para criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="f1530-214">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span>

<span data-ttu-id="f1530-215">Para obter um exemplo de Introdução do desenvolvimento do Code First, consulte [guia de Introdução ao ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="f1530-215">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span>

<span data-ttu-id="f1530-216">Para obter um exemplo mais avançado, consulte [criando um modelo de dados do Entity Framework para um aplicativo do ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="f1530-216">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="f1530-217">Observe que a API DbContext que você usa para trabalhar com dados no primeiro banco de dados é o mesmo que a API que você pode usar para trabalhar com dados no Code First.</span><span class="sxs-lookup"><span data-stu-id="f1530-217">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="f1530-218">Mesmo se você pretende usar o primeiro banco de dados, você pode aprender a lidar com cenários mais complexos, como ler e atualizar dados relacionados, controlando conflitos de simultaneidade, e assim por diante de um tutorial Code First.</span><span class="sxs-lookup"><span data-stu-id="f1530-218">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="f1530-219">A única diferença está em como o banco de dados, à classe de contexto e classes de entidade são criados.</span><span class="sxs-lookup"><span data-stu-id="f1530-219">The only difference is in how the database, context class, and entity classes are created.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f1530-220">Anterior</span><span class="sxs-lookup"><span data-stu-id="f1530-220">Previous</span></span>](enhancing-data-validation.md)

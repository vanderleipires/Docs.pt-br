---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de banco de dados | Microsoft Docs'
author: tdykstra
description: Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, por usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 3020cfa19bf12f21c6d42a77ed257595431b4e86
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="8f648-103">Implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de banco de dados</span><span class="sxs-lookup"><span data-stu-id="8f648-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="8f648-104">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8f648-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="8f648-105">Baixe o projeto Starter</span><span class="sxs-lookup"><span data-stu-id="8f648-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="8f648-106">Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="8f648-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="8f648-107">Para obter informações sobre a série, consulte [primeiro tutorial na série](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8f648-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="8f648-108">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="8f648-108">Overview</span></span>

<span data-ttu-id="8f648-109">Neste tutorial, você fazer uma alteração de banco de dados e alterações de código relacionadas, teste as alterações no Visual Studio, em seguida, implante a atualização para os ambientes de teste, preparação e produção.</span><span class="sxs-lookup"><span data-stu-id="8f648-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="8f648-110">Primeiro, o tutorial mostra como atualizar um banco de dados que é gerenciado pelo migrações do Code First e, em seguida, mais tarde mostra como atualizar um banco de dados usando o provedor dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="8f648-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="8f648-111">Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="8f648-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="8f648-112">Implantar uma atualização de banco de dados usando migrações do Code First</span><span class="sxs-lookup"><span data-stu-id="8f648-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="8f648-113">Nesta seção, você adiciona uma coluna de data de nascimento para o `Person` a classe base para o `Student` e `Instructor` entidades.</span><span class="sxs-lookup"><span data-stu-id="8f648-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="8f648-114">Em seguida, você pode atualizar a página que exibe dados de instrutor para que ele exibe a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="8f648-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="8f648-115">Por fim, você deve implantar as alterações para teste, preparação e produção.</span><span class="sxs-lookup"><span data-stu-id="8f648-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="8f648-116">Adicionar uma coluna a uma tabela no banco de dados de aplicativo</span><span class="sxs-lookup"><span data-stu-id="8f648-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="8f648-117">No *ContosoUniversity.DAL* projeto, abra *Person.cs* e adicione a propriedade a seguir no final o `Person` classe (deve haver duas chaves após ele):</span><span class="sxs-lookup"><span data-stu-id="8f648-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="8f648-118">Em seguida, atualize o `Seed` método para que ele fornece um valor para a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="8f648-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="8f648-119">Abra *Migrations\Configuration.cs* e substitua o bloco de código que começa `var instructors = new List<Instructor>` com o seguinte bloco de código que inclui informações de data de nascimento:</span><span class="sxs-lookup"><span data-stu-id="8f648-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="8f648-120">Compile a solução e, em seguida, abra o **Package Manager Console** janela.</span><span class="sxs-lookup"><span data-stu-id="8f648-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="8f648-121">Certifique-se de que ContosoUniversity.DAL ainda está selecionado como o **projeto padrão**.</span><span class="sxs-lookup"><span data-stu-id="8f648-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="8f648-122">No **Package Manager Console** janela, selecione **ContosoUniversity.DAL** como o **projeto padrão**e, em seguida, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="8f648-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="8f648-123">Quando esse comando for concluído, o Visual Studio abrirá o arquivo de classe que define o novo `DbMIgration` classe e no `Up` método, você pode ver o código que cria a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="8f648-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="8f648-124">O `Up` método cria a coluna quando você estiver implementando a alteração e o `Down` método exclui a coluna quando você está revertendo a alteração.</span><span class="sxs-lookup"><span data-stu-id="8f648-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="8f648-126">Compile a solução e, em seguida, digite o seguinte comando no **Package Manager Console** janela (Verifique se o projeto ContosoUniversity.DAL ainda está selecionado):</span><span class="sxs-lookup"><span data-stu-id="8f648-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="8f648-127">O Entity Framework executa o `Up` método e, em seguida, executa o `Seed` método.</span><span class="sxs-lookup"><span data-stu-id="8f648-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="8f648-128">Exibir a nova coluna na página instrutores</span><span class="sxs-lookup"><span data-stu-id="8f648-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="8f648-129">No projeto ContosoUniversity, abra *Instructors.aspx* e adicionar um novo campo de modelo para exibir a data de nascimento.</span><span class="sxs-lookup"><span data-stu-id="8f648-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="8f648-130">Adicione-o entre os para atribuição de contratação data e do office:</span><span class="sxs-lookup"><span data-stu-id="8f648-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="8f648-131">(Se o recuo de código obtém fora de sincronia, você pode pressionar CTRL-K e CTRL-D para reformatar automaticamente o arquivo.)</span><span class="sxs-lookup"><span data-stu-id="8f648-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="8f648-132">Execute o aplicativo e clique no **instrutores** link.</span><span class="sxs-lookup"><span data-stu-id="8f648-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="8f648-133">Quando a página for carregada, você verá que ele tem o novo campo de data de nascimento.</span><span class="sxs-lookup"><span data-stu-id="8f648-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Página de instrutores com data de nascimento](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="8f648-135">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="8f648-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="8f648-136">Implantar a atualização de banco de dados</span><span class="sxs-lookup"><span data-stu-id="8f648-136">Deploy the database update</span></span>

1. <span data-ttu-id="8f648-137">Em **Solution Explorer** selecione o projeto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="8f648-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="8f648-138">No **Web um clique em publicar** barra de ferramentas, clique no **teste** perfil de publicação e, em seguida, clique em **Publicar Web**.</span><span class="sxs-lookup"><span data-stu-id="8f648-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="8f648-139">(Se a barra de ferramentas estiver desabilitada, selecione o projeto de ContosoUniversity na **Solution Explorer**.)</span><span class="sxs-lookup"><span data-stu-id="8f648-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="8f648-140">O Visual Studio implanta o aplicativo atualizado e o navegador é aberto para a home page.</span><span class="sxs-lookup"><span data-stu-id="8f648-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="8f648-141">Execute o **instrutores** página para verificar se a atualização foi implantada com êxito.</span><span class="sxs-lookup"><span data-stu-id="8f648-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="8f648-142">Quando o aplicativo tentar acessar o banco de dados para essa página, Code First para atualizar o esquema de banco de dados e executa o `Seed` método.</span><span class="sxs-lookup"><span data-stu-id="8f648-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="8f648-143">Quando a página é exibida, você verá o esperado **data de nascimento** coluna com datas nele.</span><span class="sxs-lookup"><span data-stu-id="8f648-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="8f648-144">No **Web um clique em publicar** barra de ferramentas, clique no **preparo** perfil de publicação e, em seguida, clique em **Publicar Web**.</span><span class="sxs-lookup"><span data-stu-id="8f648-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="8f648-145">Execute o **instrutores** página no preparo para verificar se a atualização foi implantada com êxito.</span><span class="sxs-lookup"><span data-stu-id="8f648-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="8f648-146">No **Web um clique em publicar** barra de ferramentas, clique no **produção** perfil de publicação e, em seguida, clique em **Publicar Web**.</span><span class="sxs-lookup"><span data-stu-id="8f648-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="8f648-147">Execute o **instrutores** página em produção para verificar se a atualização foi implantada com êxito.</span><span class="sxs-lookup"><span data-stu-id="8f648-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="8f648-148">Para uma atualização do aplicativo real de produção que inclui uma banco de dados alteração levaria normalmente também o aplicativo offline durante a implantação usando *aplicativo\_offline.htm*, como você viu no tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="8f648-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="8f648-149">Implantar uma atualização de banco de dados usando o provedor dbDacFx</span><span class="sxs-lookup"><span data-stu-id="8f648-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="8f648-150">Nesta seção, você adiciona um *comentários* coluna para o *usuário* de tabela no banco de dados de associação e criar uma página que lhe permite exibir e editar comentários para cada usuário.</span><span class="sxs-lookup"><span data-stu-id="8f648-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="8f648-151">Em seguida, implante as alterações para teste, preparação e produção.</span><span class="sxs-lookup"><span data-stu-id="8f648-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="8f648-152">Adicionar uma coluna a uma tabela no banco de dados de associação</span><span class="sxs-lookup"><span data-stu-id="8f648-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="8f648-153">No Visual Studio, abra **Pesquisador de objetos do SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="8f648-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="8f648-154">Expanda **(localdb) \v11.0**, expanda **bancos de dados**, expanda **aspnet ContosoUniversity** (não **aspnet-ContosoUniversity-Prod**) e, em seguida, expanda **tabelas**.</span><span class="sxs-lookup"><span data-stu-id="8f648-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="8f648-155">Se você não vir **\v11.0 (localdb)** sob o **do SQL Server** nó, com o botão direito do **do SQL Server** nó e clique em **adicionar SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="8f648-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="8f648-156">No **conectar ao servidor** caixa de diálogo Inserir *(localdb) \v11.0* como o **nome do servidor**e, em seguida, clique em **conectar**.</span><span class="sxs-lookup"><span data-stu-id="8f648-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="8f648-157">Se você não vir **aspnet ContosoUniversity**, execute o projeto e faça logon usando o *admin* credenciais (a senha é *devpwd*) e, em seguida, atualize o  **Pesquisador de objetos do SQL Server** janela.</span><span class="sxs-lookup"><span data-stu-id="8f648-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="8f648-158">Clique com botão direito do **usuários** de tabela e, em seguida, clique em **Designer de exibição**.</span><span class="sxs-lookup"><span data-stu-id="8f648-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Designer de exibição SSOX](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="8f648-160">No designer, adicione um *comentários* coluna e torná-lo *nvarchar (128)* e anulável e, em seguida, clique em **atualização**.</span><span class="sxs-lookup"><span data-stu-id="8f648-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Adicionando a coluna Comments](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="8f648-162">No **atualizações de banco de dados de visualização** , clique em **Atualizar banco de dados**.</span><span class="sxs-lookup"><span data-stu-id="8f648-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Visualizar atualizações de banco de dados](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="8f648-164">Criar uma página para exibir e editar a nova coluna</span><span class="sxs-lookup"><span data-stu-id="8f648-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="8f648-165">Em **Solution Explorer**, com o botão direito do **conta** pasta no projeto ContosoUniversity, clique em **adicionar**e, em seguida, clique em **Novo Item** .</span><span class="sxs-lookup"><span data-stu-id="8f648-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="8f648-166">Criar um novo **página mestra usando dos Web formulário** e nomeie-o *UserInfo.aspx*.</span><span class="sxs-lookup"><span data-stu-id="8f648-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="8f648-167">Aceite o padrão *Site.Master* arquivo como a página mestra.</span><span class="sxs-lookup"><span data-stu-id="8f648-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="8f648-168">Copie a seguinte marcação para o `MainContent` `Content` elemento (a última do 3 `Content` elementos):</span><span class="sxs-lookup"><span data-stu-id="8f648-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="8f648-169">Clique com botão direito do *UserInfo.aspx* página e clique em **exibir no navegador**.</span><span class="sxs-lookup"><span data-stu-id="8f648-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="8f648-170">Faça logon com sua *admin* as credenciais do usuário (a senha é *devpwd*) e adicionar alguns comentários a um usuário para verificar se a página funciona corretamente.</span><span class="sxs-lookup"><span data-stu-id="8f648-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Página UserInfo](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="8f648-172">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="8f648-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="8f648-173">Implantar a atualização de banco de dados</span><span class="sxs-lookup"><span data-stu-id="8f648-173">Deploy the database update</span></span>

<span data-ttu-id="8f648-174">Para implantar usando o provedor dbDacFx, basta selecionar o **Atualizar banco de dados** opção no perfil de publicação.</span><span class="sxs-lookup"><span data-stu-id="8f648-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="8f648-175">No entanto, para a implantação inicial quando você usar essa opção é também configurado para executar alguns scripts SQL adicionais: esses são ainda no perfil e você precisa impedir a execução novamente.</span><span class="sxs-lookup"><span data-stu-id="8f648-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="8f648-176">Abra o **Publicar Web** assistente clicando duas vezes no projeto ContosoUniversity e clicando em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="8f648-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="8f648-177">Selecione o **teste** perfil.</span><span class="sxs-lookup"><span data-stu-id="8f648-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="8f648-178">Clique o **configurações** guia.</span><span class="sxs-lookup"><span data-stu-id="8f648-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="8f648-179">Em **DefaultConnection**, selecione **Atualizar banco de dados**.</span><span class="sxs-lookup"><span data-stu-id="8f648-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="8f648-180">Desabilite os scripts adicionais que você configurou para executar a implantação inicial:</span><span class="sxs-lookup"><span data-stu-id="8f648-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="8f648-181">Clique em **configurar atualizações de banco de dados**.</span><span class="sxs-lookup"><span data-stu-id="8f648-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="8f648-182">No **configurar atualizações de banco de dados** caixa de diálogo, desmarque as caixas de seleção ao lado de *Grant.sql* e *aspnet de dados de dev.sql*.</span><span class="sxs-lookup"><span data-stu-id="8f648-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="8f648-183">Clique em **Fechar**.</span><span class="sxs-lookup"><span data-stu-id="8f648-183">Click **Close**.</span></span>
6. <span data-ttu-id="8f648-184">Clique o **visualização** guia.</span><span class="sxs-lookup"><span data-stu-id="8f648-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="8f648-185">Em **bancos de dados** e à direita do **DefaultConnection**, clique no **banco de dados de visualização** link.</span><span class="sxs-lookup"><span data-stu-id="8f648-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Visualização de banco de dados](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="8f648-187">A janela de visualização mostra o script será executado no banco de dados de destino para tornar esse esquema de banco de dados corresponde ao esquema do banco de dados de origem.</span><span class="sxs-lookup"><span data-stu-id="8f648-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="8f648-188">O script inclui um comando ALTER TABLE que adiciona a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="8f648-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="8f648-189">Fechar o **visualização de banco de dados** caixa de diálogo e clique **publicar**.</span><span class="sxs-lookup"><span data-stu-id="8f648-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="8f648-190">O Visual Studio implanta o aplicativo atualizado e o navegador é aberto para a home page.</span><span class="sxs-lookup"><span data-stu-id="8f648-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="8f648-191">Execute a página UserInfo (adicionar *Account/UserInfo.aspx* para a URL da home page) para verificar se a atualização foi implantada com êxito.</span><span class="sxs-lookup"><span data-stu-id="8f648-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="8f648-192">Você precisará fazer logon digitando *admin* e *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="8f648-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="8f648-193">Dados em tabelas não são implantados por padrão, e você não configurou um script de implantação de dados em execução, para que você não encontrará o comentário que você adicionou no desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="8f648-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="8f648-194">Você pode adicionar um novo comentário agora no preparo para verificar se a alteração foi implantada no banco de dados e a página está funcionando corretamente.</span><span class="sxs-lookup"><span data-stu-id="8f648-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="8f648-195">Siga o mesmo procedimento para implantar em preparação e produção.</span><span class="sxs-lookup"><span data-stu-id="8f648-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="8f648-196">Não se esqueça de desativar os scripts adicionais.</span><span class="sxs-lookup"><span data-stu-id="8f648-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="8f648-197">A única diferença em comparação comparada o perfil de teste é que você desativará o somente um script na migração de dados e perfis de produção porque eles foram configurados para executar apenas *aspnet de produção de data.sql*.</span><span class="sxs-lookup"><span data-stu-id="8f648-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="8f648-198">As credenciais para preparação e produção são prodpwd e o administrador.</span><span class="sxs-lookup"><span data-stu-id="8f648-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="8f648-199">Para uma atualização do aplicativo real de produção que inclui uma banco de dados alteração levaria normalmente também o aplicativo offline durante a implantação, carregando *aplicativo\_offline.htm* antes de publicar e excluí-lo Depois disso, como você viu na [tutorial anterior](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="8f648-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="8f648-200">Resumo</span><span class="sxs-lookup"><span data-stu-id="8f648-200">Summary</span></span>

<span data-ttu-id="8f648-201">Agora que você implantou uma atualização de aplicativo que incluía uma alteração de banco de dados usando o provedor dbDacFx e migrações do Code First.</span><span class="sxs-lookup"><span data-stu-id="8f648-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Página de instrutores com data de nascimento](deploying-a-database-update/_static/image8.png)

![Página UserInfo](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="8f648-204">O seguinte tutorial mostra como executar as implantações por meio da linha de comando.</span><span class="sxs-lookup"><span data-stu-id="8f648-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8f648-205">[Anterior](deploying-a-code-update.md)
> [Próximo](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="8f648-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>

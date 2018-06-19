---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização do banco de dados - 9 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0a00f9d3ed284ebbc1d83c1b5696436e5ba00f4b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30884880"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a><span data-ttu-id="7c664-103">Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização do banco de dados - 9 de 12</span><span class="sxs-lookup"><span data-stu-id="7c664-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a Database Update - 9 of 12</span></span>
====================
<span data-ttu-id="7c664-104">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7c664-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="7c664-105">Baixe o projeto Starter</span><span class="sxs-lookup"><span data-stu-id="7c664-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="7c664-106">Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web.</span><span class="sxs-lookup"><span data-stu-id="7c664-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="7c664-107">Se você instalar a atualização de publicação na Web, você também pode usar o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="7c664-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="7c664-108">Para obter uma introdução à série, consulte [primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="7c664-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="7c664-109">Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server diferente do SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7c664-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="7c664-110">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="7c664-110">Overview</span></span>

<span data-ttu-id="7c664-111">Neste tutorial, você fazer uma alteração de banco de dados e alterações de código relacionadas, teste as alterações no Visual Studio, em seguida, implante a atualização para ambientes de teste e de produção.</span><span class="sxs-lookup"><span data-stu-id="7c664-111">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to both the test and production environments.</span></span>

<span data-ttu-id="7c664-112">Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="7c664-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="7c664-113">Adicionar uma nova coluna a uma tabela</span><span class="sxs-lookup"><span data-stu-id="7c664-113">Adding a New Column to a Table</span></span>

<span data-ttu-id="7c664-114">Nesta seção, você adiciona uma coluna de data de nascimento para o `Person` a classe base para o `Student` e `Instructor` entidades.</span><span class="sxs-lookup"><span data-stu-id="7c664-114">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="7c664-115">Em seguida, você pode atualizar a página que exibe dados de instrutor para que ele exibe a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="7c664-115">Then you update the page that displays instructor data so that it displays the new column.</span></span>

<span data-ttu-id="7c664-116">No *ContosoUniversity.DAL* projeto, abra *Person.cs* e adicione a propriedade a seguir no final o `Person` classe (deve haver duas chaves após ele):</span><span class="sxs-lookup"><span data-stu-id="7c664-116">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

<span data-ttu-id="7c664-117">Em seguida, atualize o método de propagação para que ele forneça um valor para a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="7c664-117">Next, update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="7c664-118">Abra *Migrations\Configuration.cs* e substitua o bloco de código que começa `var instructors = new List<Instructor>` com o seguinte bloco de código que inclui informações de data de nascimento:</span><span class="sxs-lookup"><span data-stu-id="7c664-118">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

<span data-ttu-id="7c664-119">No projeto ContosoUniversity, abra *Instructors.aspx* e adicionar um novo campo de modelo para exibir a data de nascimento.</span><span class="sxs-lookup"><span data-stu-id="7c664-119">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="7c664-120">Adicione-o entre os para atribuição de contratação data e do office:</span><span class="sxs-lookup"><span data-stu-id="7c664-120">Add it between the ones for hire date and office assignment:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

<span data-ttu-id="7c664-121">(Se o recuo de código obtém fora de sincronia, você pode pressionar CTRL-K e CTRL-D para reformatar automaticamente o arquivo.)</span><span class="sxs-lookup"><span data-stu-id="7c664-121">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>

<span data-ttu-id="7c664-122">Compile a solução e, em seguida, abra o **Package Manager Console** janela.</span><span class="sxs-lookup"><span data-stu-id="7c664-122">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="7c664-123">Certifique-se de que ContosoUniversity.DAL ainda está selecionado como o **projeto padrão**.</span><span class="sxs-lookup"><span data-stu-id="7c664-123">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>

<span data-ttu-id="7c664-124">No **Package Manager Console** janela, selecione **ContosoUniversity.DAL** como o **projeto padrão**e, em seguida, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="7c664-124">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

<span data-ttu-id="7c664-125">Quando esse comando for concluído, o Visual Studio abrirá o arquivo de classe que define o novo `DbMIgration` classe e no `Up` método, você pode ver o código que cria a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="7c664-125">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span>

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

<span data-ttu-id="7c664-127">Compile a solução e, em seguida, digite o seguinte comando no **Package Manager Console** janela (Verifique se o projeto ContosoUniversity.DAL ainda está selecionado):</span><span class="sxs-lookup"><span data-stu-id="7c664-127">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

<span data-ttu-id="7c664-128">Quando o comando for concluído, execute o aplicativo e selecione a página de professores.</span><span class="sxs-lookup"><span data-stu-id="7c664-128">When the command finishes, run the application and select the Instructors page.</span></span> <span data-ttu-id="7c664-129">Quando a página for carregada, você verá que ele tem o novo campo de data de nascimento.</span><span class="sxs-lookup"><span data-stu-id="7c664-129">When the page loads, you see that it has the new birth date field.</span></span>

<span data-ttu-id="7c664-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="7c664-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="7c664-131">Implantação da atualização do banco de dados para o ambiente de teste</span><span class="sxs-lookup"><span data-stu-id="7c664-131">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="7c664-132">Em **Solution Explorer** selecione o projeto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="7c664-132">In **Solution Explorer** select the ContosoUniversity project.</span></span>

<span data-ttu-id="7c664-133">No **Web um clique em publicar** barra de ferramentas, selecione o **teste** perfil de publicação e, em seguida, clique em **Publicar Web**.</span><span class="sxs-lookup"><span data-stu-id="7c664-133">In the **Web One Click Publish** toolbar, select the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="7c664-134">(Se a barra de ferramentas estiver desabilitada, selecione o projeto de ContosoUniversity na **Solution Explorer**.)</span><span class="sxs-lookup"><span data-stu-id="7c664-134">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

<span data-ttu-id="7c664-135">O Visual Studio implanta o aplicativo atualizado e o navegador é aberto para a home page.</span><span class="sxs-lookup"><span data-stu-id="7c664-135">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span> <span data-ttu-id="7c664-136">Execute a página instrutores para verificar se a atualização foi implantada com êxito.</span><span class="sxs-lookup"><span data-stu-id="7c664-136">Run the Instructors page to verify that the update was successfully deployed.</span></span> <span data-ttu-id="7c664-137">Quando o aplicativo tentar acessar o banco de dados para essa página, Code First para atualizar o esquema de banco de dados e executa o `Seed` método.</span><span class="sxs-lookup"><span data-stu-id="7c664-137">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="7c664-138">Quando a página é exibida, você verá o esperado **data de nascimento** coluna com datas nele.</span><span class="sxs-lookup"><span data-stu-id="7c664-138">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>

<span data-ttu-id="7c664-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="7c664-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="7c664-140">Implantação da atualização do banco de dados para o ambiente de produção</span><span class="sxs-lookup"><span data-stu-id="7c664-140">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="7c664-141">Agora, você pode implantar na produção.</span><span class="sxs-lookup"><span data-stu-id="7c664-141">You can now deploy to production.</span></span> <span data-ttu-id="7c664-142">A única diferença é que você usará *aplicativo\_offline.htm* para impedir que os usuários acessam o site e, portanto, atualizar o banco de dados enquanto você estiver implantando as alterações.</span><span class="sxs-lookup"><span data-stu-id="7c664-142">The only difference is that you'll use *app\_offline.htm* to prevent users from accessing the site and thus updating the database while you're deploying changes.</span></span> <span data-ttu-id="7c664-143">Para implantação de produção, execute as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="7c664-143">For production deployment perform the following steps:</span></span>

- <span data-ttu-id="7c664-144">Carregar o *aplicativo\_offline.htm* arquivo para o site de produção.</span><span class="sxs-lookup"><span data-stu-id="7c664-144">Upload the *app\_offline.htm* file to the production site.</span></span>
- <span data-ttu-id="7c664-145">No Visual Studio, escolha o perfil de produção no **Web um clique em publicar** barra de ferramentas e clique em **Publicar Web**.</span><span class="sxs-lookup"><span data-stu-id="7c664-145">In Visual Studio, choose the Production profile in the **Web One Click Publish** toolbar and click **Publish Web**.</span></span>
- <span data-ttu-id="7c664-146">Excluir o *aplicativo\_offline.htm* arquivo do site de produção.</span><span class="sxs-lookup"><span data-stu-id="7c664-146">Delete the *app\_offline.htm* file from the production site.</span></span>

> [!NOTE]
> <span data-ttu-id="7c664-147">Enquanto seu aplicativo estiver em uso no ambiente de produção deve implementar um plano de backup.</span><span class="sxs-lookup"><span data-stu-id="7c664-147">While your application is in use in the production environment you should be implementing a backup plan.</span></span> <span data-ttu-id="7c664-148">Ou seja, você deve ser periodicamente copiando o *escola-Prod.sdf* e *Prod.sdf aspnet* arquivos da produção do site para um local de armazenamento seguro e você deve manter várias gerações de tais backups.</span><span class="sxs-lookup"><span data-stu-id="7c664-148">That is, you should be periodically copying the *School-Prod.sdf* and *aspnet-Prod.sdf* files from the production site to a secure storage location, and you should be keeping several generations of such backups.</span></span> <span data-ttu-id="7c664-149">Quando você atualizar o banco de dados, você deve fazer uma cópia de backup de imediatamente antes da alteração.</span><span class="sxs-lookup"><span data-stu-id="7c664-149">When you update the database, you should make a backup copy from immediately before the change.</span></span> <span data-ttu-id="7c664-150">Em seguida, se você comete um erro e não Descubra até depois de implantá-lo em produção, você ainda poderá recuperar o banco de dados para o estado em que estava antes que ele se tornou corrompido.</span><span class="sxs-lookup"><span data-stu-id="7c664-150">Then, if you make a mistake and don't discover it until after you have deployed it to production, you will still be able to recover the database to the state it was in before it became corrupted.</span></span>


<span data-ttu-id="7c664-151">Quando o Visual Studio abrirá o URL da home page no navegador, o *aplicativo\_offline.htm* página é exibida.</span><span class="sxs-lookup"><span data-stu-id="7c664-151">When Visual Studio opens the home page URL in the browser, the *app\_offline.htm* page is displayed.</span></span> <span data-ttu-id="7c664-152">Depois de excluir o *aplicativo\_offline.htm* arquivo, você pode navegar para a home page novamente para verificar se a atualização foi implantada com êxito.</span><span class="sxs-lookup"><span data-stu-id="7c664-152">After you delete the *app\_offline.htm* file, you can browse to your home page again to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="7c664-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="7c664-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span></span>

<span data-ttu-id="7c664-154">Agora que você implantou uma atualização de aplicativo que incluía uma alteração de banco de dados para teste e produção.</span><span class="sxs-lookup"><span data-stu-id="7c664-154">You've now deployed an application update that included a database change to both test and production.</span></span> <span data-ttu-id="7c664-155">O seguinte tutorial mostra como migrar seu banco de dados do SQL Server Compact para o SQL Server Express e o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7c664-155">The next tutorial shows you how to migrate your database from SQL Server Compact to SQL Server Express and SQL Server.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7c664-156">[Anterior](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="7c664-156">[Previous](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
[Next](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span></span>

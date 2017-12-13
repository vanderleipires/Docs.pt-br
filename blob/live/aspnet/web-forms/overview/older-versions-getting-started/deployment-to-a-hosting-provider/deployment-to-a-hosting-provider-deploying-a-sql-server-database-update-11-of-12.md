---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: "Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização de banco de dados do SQL Server - 11 12 | Microsoft Docs"
author: tdykstra
description: "Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 898259885da8a089db296bd0f400ee8863877d08
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="4f754-103">Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização de banco de dados do SQL Server - 11 12</span><span class="sxs-lookup"><span data-stu-id="4f754-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>
====================
<span data-ttu-id="4f754-104">Por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4f754-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="4f754-105">Baixe o projeto Starter</span><span class="sxs-lookup"><span data-stu-id="4f754-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="4f754-106">Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web.</span><span class="sxs-lookup"><span data-stu-id="4f754-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="4f754-107">Se você instalar a atualização de publicação na Web, você também pode usar o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="4f754-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="4f754-108">Para obter uma introdução à série, consulte [primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="4f754-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="4f754-109">Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server diferente do SQL Server Compact e mostra como implantar o Windows Azure Web Sites, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="4f754-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="4f754-110">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="4f754-110">Overview</span></span>

<span data-ttu-id="4f754-111">Este tutorial mostra como implantar uma atualização de banco de dados em um banco de dados completo do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4f754-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="4f754-112">Como migrações do Code First faz todo o trabalho de atualização de banco de dados, o processo é quase idêntico ao que você fez para o SQL Server Compact no [implantar uma atualização de banco de dados](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="4f754-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="4f754-113">Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="4f754-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="4f754-114">Adicionar uma nova coluna a uma tabela</span><span class="sxs-lookup"><span data-stu-id="4f754-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="4f754-115">Esta seção do tutorial, você fará um banco de dados de alteração e alterações de código correspondente, testá-las no Visual Studio em preparação para implantá-los nos ambientes de teste e produção.</span><span class="sxs-lookup"><span data-stu-id="4f754-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="4f754-116">A alteração envolve a adição de um `OfficeHours` coluna para o `Instructor` entidade e exibir as novas informações no **instrutores** página da web.</span><span class="sxs-lookup"><span data-stu-id="4f754-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="4f754-117">No projeto ContosoUniversity.DAL, abra *Instructor.cs* e adicione a seguinte propriedade entre o `HireDate` e `Courses` propriedades:</span><span class="sxs-lookup"><span data-stu-id="4f754-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="4f754-118">Atualize a classe de inicializador de modo que ele propaga a nova coluna com dados de teste.</span><span class="sxs-lookup"><span data-stu-id="4f754-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="4f754-119">Abra *Migrations\Configuration.cs* e substitua o bloco de código que começa `var instructors = new List<Instructor>` com o seguinte bloco de código que inclui a nova coluna:</span><span class="sxs-lookup"><span data-stu-id="4f754-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="4f754-120">No projeto ContosoUniversity, abra *Instructors.aspx* e adicionar um novo campo de modelo para o horário de expediente antes do fechamento `</Columns>` marca na primeira `GridView` controle:</span><span class="sxs-lookup"><span data-stu-id="4f754-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="4f754-121">Compile a solução.</span><span class="sxs-lookup"><span data-stu-id="4f754-121">Build the solution.</span></span>

<span data-ttu-id="4f754-122">Abra o **Package Manager Console** janela e selecione ContosoUniversity.DAL como o **projeto padrão**.</span><span class="sxs-lookup"><span data-stu-id="4f754-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="4f754-123">Digite os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="4f754-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="4f754-124">Execute o aplicativo e selecione o **instrutores** página.</span><span class="sxs-lookup"><span data-stu-id="4f754-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="4f754-125">A página demora um pouco mais do que o normal para carregar, porque o Entity Framework recria o banco de dados e propaga-lo com dados de teste.</span><span class="sxs-lookup"><span data-stu-id="4f754-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="4f754-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4f754-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="4f754-127">Implantação da atualização do banco de dados para o ambiente de teste</span><span class="sxs-lookup"><span data-stu-id="4f754-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="4f754-128">Quando você usa as migrações do Code First, o método de implantação de uma alteração de banco de dados para o SQL Server é igual do SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="4f754-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="4f754-129">No entanto, você precisa alterar o teste de perfil de publicação, porque ele ainda está configurado para migrar do SQL Server Compact para o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4f754-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="4f754-130">A primeira etapa é remover as transformações de cadeia de caracteres de conexão que você criou no tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="4f754-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="4f754-131">Eles não são mais necessários porque você irá especificar transformações de cadeia de caracteres de conexão no perfil de publicação, como você fez antes que você configurou o **pacote/publicar SQL** guia de migração para o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4f754-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="4f754-132">Abra o *Web.Test.config* de arquivo e remover o `connectionStrings` elemento.</span><span class="sxs-lookup"><span data-stu-id="4f754-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="4f754-133">A transformação somente restante no *Web.Test.config* arquivo é para o `Environment` valor o `appSettings` elemento.</span><span class="sxs-lookup"><span data-stu-id="4f754-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="4f754-134">Agora você pode atualizar o perfil de publicação e publicar para o ambiente de teste.</span><span class="sxs-lookup"><span data-stu-id="4f754-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="4f754-135">Abra o **Publicar Web** assistente e, em seguida, alternar para o **perfil** guia.</span><span class="sxs-lookup"><span data-stu-id="4f754-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="4f754-136">Selecione o **teste** perfil de publicação.</span><span class="sxs-lookup"><span data-stu-id="4f754-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="4f754-137">Selecione o **configurações** guia.</span><span class="sxs-lookup"><span data-stu-id="4f754-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="4f754-138">Clique em **habilitar o banco de dados novas melhorias de publicação**.</span><span class="sxs-lookup"><span data-stu-id="4f754-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="4f754-139">Na caixa de cadeia de caracteres de conexão do **SchoolContext**, inserir o mesmo valor que você usou o *Web.Test.config* arquivo de transformação no tutorial anterior:</span><span class="sxs-lookup"><span data-stu-id="4f754-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="4f754-140">Selecione **executar migrações do Code First (executado na inicialização do aplicativo)**.</span><span class="sxs-lookup"><span data-stu-id="4f754-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="4f754-141">(Na sua versão do Visual Studio, a caixa de seleção deve ser rotulada **aplicar migrações do Code First**.)</span><span class="sxs-lookup"><span data-stu-id="4f754-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="4f754-142">Na caixa de cadeia de caracteres de conexão do **DefaultConnection**, inserir o mesmo valor que você usou o *Web.Test.config* arquivo de transformação no tutorial anterior:</span><span class="sxs-lookup"><span data-stu-id="4f754-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="4f754-143">Deixe **Atualizar banco de dados** desmarcada.</span><span class="sxs-lookup"><span data-stu-id="4f754-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="4f754-144">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="4f754-144">Click **Publish**.</span></span>

<span data-ttu-id="4f754-145">Visual Studio implanta as alterações de código para o ambiente de teste e abre o navegador para a home page da Contoso University.</span><span class="sxs-lookup"><span data-stu-id="4f754-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="4f754-146">Selecione a página de professores.</span><span class="sxs-lookup"><span data-stu-id="4f754-146">Select the Instructors page.</span></span>

<span data-ttu-id="4f754-147">Quando o aplicativo é executado nessa página, ele tenta acessar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f754-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="4f754-148">Migrações do Code First verifica se o banco de dados atual e localiza a que a migração mais recente não foi aplicada ainda.</span><span class="sxs-lookup"><span data-stu-id="4f754-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="4f754-149">Migrações do Code First aplica-se a migração mais recente, é executado o `Seed` método e, em seguida, a página é executada normalmente.</span><span class="sxs-lookup"><span data-stu-id="4f754-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="4f754-150">Você ver a nova coluna de horário comercial com os dados de propagação.</span><span class="sxs-lookup"><span data-stu-id="4f754-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="4f754-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="4f754-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="4f754-152">Implantação da atualização do banco de dados para o ambiente de produção</span><span class="sxs-lookup"><span data-stu-id="4f754-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="4f754-153">Você precisa alterar o perfil de publicação para o ambiente de produção também.</span><span class="sxs-lookup"><span data-stu-id="4f754-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="4f754-154">Nesse caso, você será remova o perfil atual e crie um novo com a importação de um arquivo. publishsettings atualizado.</span><span class="sxs-lookup"><span data-stu-id="4f754-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="4f754-155">O arquivo atualizado incluirá a cadeia de caracteres de conexão para o banco de dados do SQL Server em Cytanium.</span><span class="sxs-lookup"><span data-stu-id="4f754-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="4f754-156">Como você viu quando é implantado no ambiente de teste, você não precisa mais transformações de cadeia de caracteres de conexão no *Web.Production.config* arquivo de transformação.</span><span class="sxs-lookup"><span data-stu-id="4f754-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="4f754-157">Abrir arquivo e remover o `connectionStrings` elemento.</span><span class="sxs-lookup"><span data-stu-id="4f754-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="4f754-158">As transformações restantes são para o `Environment` valor o `appSettings` elemento e o `location` elemento que restringe o acesso a relatórios de erros do Elmah.</span><span class="sxs-lookup"><span data-stu-id="4f754-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="4f754-159">Antes de criar um novo perfil de publicação para produção, baixar um arquivo. publishsettings atualizada da mesma forma que você fez anteriormente no [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="4f754-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="4f754-160">(No painel de controle Cytanium, clique em **Sites da Web**e, em seguida, clique no **contosouniversity.com** site.</span><span class="sxs-lookup"><span data-stu-id="4f754-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="4f754-161">Selecione o **Web Publishing** guia e, em seguida, clique em **baixar perfil de publicação para este site**.) O motivo pelo qual que você está fazendo isso é obter a cadeia de caracteres de conexão de banco de dados no arquivo. publishsettings.</span><span class="sxs-lookup"><span data-stu-id="4f754-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="4f754-162">A cadeia de caracteres de conexão não disponível na primeira vez em que você baixou o arquivo, porque você estava usando o SQL Server Compact e não tivesse criado o banco de dados do SQL Server no Cytanium ainda.</span><span class="sxs-lookup"><span data-stu-id="4f754-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="4f754-163">Agora você pode atualizar o perfil de publicação e publicar no ambiente de produção.</span><span class="sxs-lookup"><span data-stu-id="4f754-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="4f754-164">Abra o **Publicar Web** assistente e, em seguida, alternar para o **perfil** guia.</span><span class="sxs-lookup"><span data-stu-id="4f754-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="4f754-165">Clique em **gerenciar perfis**e, em seguida, exclua o perfil de produção.</span><span class="sxs-lookup"><span data-stu-id="4f754-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="4f754-166">Fechar o **Publicar Web** Assistente para salvar essa alteração.</span><span class="sxs-lookup"><span data-stu-id="4f754-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="4f754-167">Abra o **Publicar Web** novamente o assistente e depois clique em **importação**.</span><span class="sxs-lookup"><span data-stu-id="4f754-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="4f754-168">Sobre o **Conexão** , altere **URL de destino** para o valor apropriado se você estiver usando uma URL temporária.</span><span class="sxs-lookup"><span data-stu-id="4f754-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="4f754-169">Clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="4f754-169">Click **Next**.</span></span>

<span data-ttu-id="4f754-170">Sobre o **configurações** , clique em **habilitar o banco de dados novas melhorias de publicação**.</span><span class="sxs-lookup"><span data-stu-id="4f754-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="4f754-171">Na lista suspensa de cadeia de caracteres de conexão para **SchoolContext**, selecione a cadeia de caracteres de conexão Cytanium.</span><span class="sxs-lookup"><span data-stu-id="4f754-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="4f754-173">Selecione **migrações executar Code First (executado na inicialização do aplicativo)**.</span><span class="sxs-lookup"><span data-stu-id="4f754-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="4f754-174">Na lista suspensa de cadeia de caracteres de conexão para **DefaultConnection**, selecione a cadeia de caracteres de conexão Cytanium.</span><span class="sxs-lookup"><span data-stu-id="4f754-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="4f754-175">Selecione o **perfil** , clique em **gerenciar perfis**e renomeie o perfil de "contosouniversity.com - a implantação da Web" para "Produção".</span><span class="sxs-lookup"><span data-stu-id="4f754-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="4f754-176">Feche o perfil de publicação para salvar a alteração e abra-o novamente.</span><span class="sxs-lookup"><span data-stu-id="4f754-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="4f754-177">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="4f754-177">Click **Publish**.</span></span> <span data-ttu-id="4f754-178">(Para um site de produção real, copie *aplicativo\_offline.htm* para a produção e put na pasta do seu projeto antes de publicar, em seguida, removê-lo quando a implantação for concluída.)</span><span class="sxs-lookup"><span data-stu-id="4f754-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="4f754-179">Visual Studio implanta as alterações de código para o ambiente de teste e abre o navegador para a home page da Contoso University.</span><span class="sxs-lookup"><span data-stu-id="4f754-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="4f754-180">Selecione a página de professores.</span><span class="sxs-lookup"><span data-stu-id="4f754-180">Select the Instructors page.</span></span>

<span data-ttu-id="4f754-181">Migrações do Code First atualiza o banco de dados da mesma maneira que no ambiente de teste.</span><span class="sxs-lookup"><span data-stu-id="4f754-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="4f754-182">Você ver a nova coluna de horário comercial com os dados de propagação.</span><span class="sxs-lookup"><span data-stu-id="4f754-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="4f754-184">Você agora implantou com êxito uma atualização de aplicativo que incluía uma alteração de banco de dados, usando um banco de dados do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4f754-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="4f754-185">Mais informações</span><span class="sxs-lookup"><span data-stu-id="4f754-185">More Information</span></span>

<span data-ttu-id="4f754-186">Isso conclui esta série de tutoriais sobre como implantar um aplicativo da web ASP.NET em um provedor de hospedagem de terceiros.</span><span class="sxs-lookup"><span data-stu-id="4f754-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="4f754-187">Para obter mais informações sobre qualquer um dos tópicos abordados nesses tutoriais, consulte o [mapa de conteúdo de implantação do ASP.NET](https://msdn.microsoft.com/en-us/library/bb386521(v=vs.110).aspx) no site do MSDN.</span><span class="sxs-lookup"><span data-stu-id="4f754-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/en-us/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="4f754-188">Confirmações</span><span class="sxs-lookup"><span data-stu-id="4f754-188">Acknowledgements</span></span>

<span data-ttu-id="4f754-189">Gostaria de agradecer seguintes pessoas que fizeram contribuições significativas para o conteúdo desta série tutorial:</span><span class="sxs-lookup"><span data-stu-id="4f754-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="4f754-190">Alberto Poblacion, MVP &amp; MCT, Espanha</span><span class="sxs-lookup"><span data-stu-id="4f754-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="4f754-191">Jarod Ferguson, MVP de desenvolvimento de plataforma de dados, EUA</span><span class="sxs-lookup"><span data-stu-id="4f754-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="4f754-192">Adversos Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="4f754-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="4f754-193">Kristina Olson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="4f754-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="4f754-194">Mike Pope, Microsoft</span><span class="sxs-lookup"><span data-stu-id="4f754-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="4f754-195">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="4f754-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="4f754-196">Raffaele Rialdi, Itália</span><span class="sxs-lookup"><span data-stu-id="4f754-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="4f754-197">Rick Anderson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="4f754-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="4f754-198">[Hashimi sayed, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="4f754-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="4f754-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="4f754-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="4f754-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="4f754-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="4f754-201">Srđan Božović, Sérvia</span><span class="sxs-lookup"><span data-stu-id="4f754-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="4f754-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="4f754-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4f754-203">[Anterior](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Próximo](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="4f754-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>

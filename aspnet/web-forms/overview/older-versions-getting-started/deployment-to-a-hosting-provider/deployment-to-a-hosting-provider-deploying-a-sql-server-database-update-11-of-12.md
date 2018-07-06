---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização de banco de dados do SQL Server - 11 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: c744bc54c8ce12820d1e1388ac7ab2db814ff031
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816620"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="61791-103">Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização de banco de dados do SQL Server - 11 12</span><span class="sxs-lookup"><span data-stu-id="61791-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>
====================
<span data-ttu-id="61791-104">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="61791-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="61791-105">Baixe o projeto inicial</span><span class="sxs-lookup"><span data-stu-id="61791-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="61791-106">Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web.</span><span class="sxs-lookup"><span data-stu-id="61791-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="61791-107">Você também pode usar o Visual Studio 2010 se você instalar a atualização de publicação na Web.</span><span class="sxs-lookup"><span data-stu-id="61791-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="61791-108">Para obter uma introdução à série, consulte [o primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="61791-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="61791-109">Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server que não seja o SQL Server Compact e mostra como implantar o Windows Azure Web Sites, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="61791-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="61791-110">Visão geral</span><span class="sxs-lookup"><span data-stu-id="61791-110">Overview</span></span>

<span data-ttu-id="61791-111">Este tutorial mostra como implantar uma atualização de banco de dados em um banco de dados completo do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="61791-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="61791-112">Como as migrações Code First faz todo o trabalho de atualização do banco de dados, o processo é quase idêntico ao que você fez para o SQL Server Compact na [Implantando uma atualização de banco de dados](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="61791-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="61791-113">Lembrete: Se você receber uma mensagem de erro ou se algo não funciona ao percorrer o tutorial, não se esqueça de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="61791-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="61791-114">Adicionando uma nova coluna a uma tabela</span><span class="sxs-lookup"><span data-stu-id="61791-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="61791-115">Nesta seção do tutorial, você fará um banco de dados altere e alterações de código correspondente, em seguida, testá-los no Visual Studio em preparação para implantá-los para ambientes de teste e produção.</span><span class="sxs-lookup"><span data-stu-id="61791-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="61791-116">A alteração envolve a adição de um `OfficeHours` coluna para o `Instructor` entidade e exibindo as novas informações na **instrutores** página da web.</span><span class="sxs-lookup"><span data-stu-id="61791-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="61791-117">No projeto ContosoUniversity.DAL, abra *Instructor.cs* e adicione a seguinte propriedade entre o `HireDate` e `Courses` propriedades:</span><span class="sxs-lookup"><span data-stu-id="61791-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="61791-118">Atualize a classe de inicializador para que ele propaga a nova coluna com dados de teste.</span><span class="sxs-lookup"><span data-stu-id="61791-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="61791-119">Abra *Migrations\Configuration.cs* e substitua o bloco de código começa `var instructors = new List<Instructor>` com o seguinte bloco de código que inclui a nova coluna:</span><span class="sxs-lookup"><span data-stu-id="61791-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="61791-120">No projeto ContosoUniversity, abra *Instructors.aspx* e adicione um novo campo de modelo para o horário de expediente antes do fechamento `</Columns>` marca no primeiro `GridView` controle:</span><span class="sxs-lookup"><span data-stu-id="61791-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="61791-121">Compile a solução.</span><span class="sxs-lookup"><span data-stu-id="61791-121">Build the solution.</span></span>

<span data-ttu-id="61791-122">Abra o **Package Manager Console** janela e selecione ContosoUniversity.DAL como o **projeto padrão**.</span><span class="sxs-lookup"><span data-stu-id="61791-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="61791-123">Insira os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="61791-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="61791-124">Execute o aplicativo e selecione o **instrutores** página.</span><span class="sxs-lookup"><span data-stu-id="61791-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="61791-125">A página demora um pouco mais do que o normal para carregar, porque o Entity Framework recria o banco de dados e propagará com os dados de teste.</span><span class="sxs-lookup"><span data-stu-id="61791-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="61791-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="61791-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="61791-127">Implantar a atualização de banco de dados para o ambiente de teste</span><span class="sxs-lookup"><span data-stu-id="61791-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="61791-128">Quando você usar migrações do Code First, o método para implantar uma alteração de banco de dados para o SQL Server é a mesma do SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="61791-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="61791-129">No entanto, você precisará alterar o teste de perfil de publicação porque ele ainda está configurado para migrar do SQL Server Compact para o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="61791-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="61791-130">A primeira etapa é remover as transformações de cadeia de caracteres de conexão que você criou no tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="61791-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="61791-131">Eles não são mais necessários porque você especificará as transformações de cadeia de caracteres de conexão no perfil de publicação, como você fez antes que você configurou a **empacotar/publicar SQL** guia para a migração para o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="61791-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="61791-132">Abra o *Web.Test.config* do arquivo e remover o `connectionStrings` elemento.</span><span class="sxs-lookup"><span data-stu-id="61791-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="61791-133">A única transformação restante no *Web.Test.config* arquivo destina-se a `Environment` o valor o `appSettings` elemento.</span><span class="sxs-lookup"><span data-stu-id="61791-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="61791-134">Agora você pode atualizar o perfil de publicação e publicar no ambiente de teste.</span><span class="sxs-lookup"><span data-stu-id="61791-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="61791-135">Abra o **Publicar Web** assistente e, em seguida, alterne para o **perfil** guia.</span><span class="sxs-lookup"><span data-stu-id="61791-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="61791-136">Selecione o **teste** perfil de publicação.</span><span class="sxs-lookup"><span data-stu-id="61791-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="61791-137">Selecione o **configurações** guia.</span><span class="sxs-lookup"><span data-stu-id="61791-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="61791-138">Clique em **habilitar o novo banco de dados de publicação melhorias**.</span><span class="sxs-lookup"><span data-stu-id="61791-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="61791-139">Na caixa de cadeia de caracteres de conexão para **SchoolContext**, insira o mesmo valor que você usou na *Web.Test.config* arquivo de transformação no tutorial anterior:</span><span class="sxs-lookup"><span data-stu-id="61791-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="61791-140">Selecione **executar migrações do Code First (executado na inicialização do aplicativo)**.</span><span class="sxs-lookup"><span data-stu-id="61791-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="61791-141">(Na sua versão do Visual Studio, a caixa de seleção deve ser rotulada **aplicar migrações do Code First**.)</span><span class="sxs-lookup"><span data-stu-id="61791-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="61791-142">Na caixa de cadeia de caracteres de conexão para **DefaultConnection**, insira o mesmo valor que você usou na *Web.Test.config* arquivo de transformação no tutorial anterior:</span><span class="sxs-lookup"><span data-stu-id="61791-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="61791-143">Deixe **Atualizar banco de dados** desmarcada.</span><span class="sxs-lookup"><span data-stu-id="61791-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="61791-144">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="61791-144">Click **Publish**.</span></span>

<span data-ttu-id="61791-145">Visual Studio implanta as alterações de código para o ambiente de teste e abre o navegador para a home page do Contoso University.</span><span class="sxs-lookup"><span data-stu-id="61791-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="61791-146">Selecione a página instrutores.</span><span class="sxs-lookup"><span data-stu-id="61791-146">Select the Instructors page.</span></span>

<span data-ttu-id="61791-147">Quando o aplicativo é executado nessa página, ele tenta acessar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="61791-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="61791-148">Migrações do Code First verifica se o banco de dados atual e localiza a que a migração mais recente ainda não foi aplicada.</span><span class="sxs-lookup"><span data-stu-id="61791-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="61791-149">Migrações do Code First aplica-se a migração mais recente, será executado o `Seed` método e, em seguida, a página é executada normalmente.</span><span class="sxs-lookup"><span data-stu-id="61791-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="61791-150">Você ver a nova coluna de horas de trabalho com os dados propagados.</span><span class="sxs-lookup"><span data-stu-id="61791-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="61791-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="61791-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="61791-152">Implantar a atualização de banco de dados para o ambiente de produção</span><span class="sxs-lookup"><span data-stu-id="61791-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="61791-153">Você precisará alterar o perfil de publicação para o ambiente de produção também.</span><span class="sxs-lookup"><span data-stu-id="61791-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="61791-154">Nesse caso, você remover o perfil existente e criar um novo importando um arquivo. publishsettings atualizado.</span><span class="sxs-lookup"><span data-stu-id="61791-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="61791-155">O arquivo atualizado incluirá a cadeia de caracteres de conexão para o banco de dados do SQL Server em Cytanium.</span><span class="sxs-lookup"><span data-stu-id="61791-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="61791-156">Como você viu quando você implantou no ambiente de teste, você não precisa mais transformações de cadeia de caracteres de conexão na *Web.Production.config* arquivo de transformação.</span><span class="sxs-lookup"><span data-stu-id="61791-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="61791-157">Abrir arquivo e remover o `connectionStrings` elemento.</span><span class="sxs-lookup"><span data-stu-id="61791-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="61791-158">As transformações restantes são para o `Environment` o valor a `appSettings` elemento e o `location` elemento que restringe o acesso a relatórios de erros do Elmah.</span><span class="sxs-lookup"><span data-stu-id="61791-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="61791-159">Antes de criar um novo perfil de publicação para produção, baixar um arquivo. publishsettings atualizada da mesma forma que você fez anteriormente na [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="61791-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="61791-160">(No painel de controle Cytanium, clique em **Sites da Web**e, em seguida, clique no **contosouniversity.com** site.</span><span class="sxs-lookup"><span data-stu-id="61791-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="61791-161">Selecione o **Web Publishing** guia e, em seguida, clique em **baixar perfil de publicação para este site**.) É o motivo pelo qual que você estiver fazendo isso acompanhar a cadeia de caracteres de conexão de banco de dados no arquivo. publishsettings.</span><span class="sxs-lookup"><span data-stu-id="61791-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="61791-162">A cadeia de caracteres de conexão não disponível na primeira vez em que você baixou o arquivo, porque você estava usando o SQL Server Compact e ainda não tenha criado o banco de dados do SQL Server em Cytanium ainda.</span><span class="sxs-lookup"><span data-stu-id="61791-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="61791-163">Agora você pode atualizar o perfil de publicação e publicar no ambiente de produção.</span><span class="sxs-lookup"><span data-stu-id="61791-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="61791-164">Abra o **Publicar Web** assistente e, em seguida, alterne para o **perfil** guia.</span><span class="sxs-lookup"><span data-stu-id="61791-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="61791-165">Clique em **gerenciar perfis**e, em seguida, exclua o perfil de produção.</span><span class="sxs-lookup"><span data-stu-id="61791-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="61791-166">Fechar o **publicar na Web** Assistente para salvar essa alteração.</span><span class="sxs-lookup"><span data-stu-id="61791-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="61791-167">Abra o **Publicar Web** novamente o assistente e clique **importação**.</span><span class="sxs-lookup"><span data-stu-id="61791-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="61791-168">Sobre o **Conexão** , altere **URL de destino** para o valor apropriado, se você estiver usando uma URL temporária.</span><span class="sxs-lookup"><span data-stu-id="61791-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="61791-169">Clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="61791-169">Click **Next**.</span></span>

<span data-ttu-id="61791-170">Sobre o **as configurações** , clique em **habilitar novo melhorias de publicação do banco de dados**.</span><span class="sxs-lookup"><span data-stu-id="61791-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="61791-171">Na lista suspensa de cadeia de caracteres de conexão para **SchoolContext**, selecione a cadeia de caracteres de conexão Cytanium.</span><span class="sxs-lookup"><span data-stu-id="61791-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="61791-173">Selecione **migrações Execute Code First (executado na inicialização do aplicativo)**.</span><span class="sxs-lookup"><span data-stu-id="61791-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="61791-174">Na lista suspensa de cadeia de caracteres de conexão para **DefaultConnection**, selecione a cadeia de caracteres de conexão Cytanium.</span><span class="sxs-lookup"><span data-stu-id="61791-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="61791-175">Selecione o **perfil** , clique em **gerenciar perfis**e renomeie o perfil de "contosouniversity.com - a implantação da Web" para "Produção".</span><span class="sxs-lookup"><span data-stu-id="61791-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="61791-176">Feche o perfil de publicação para salvar a alteração, em seguida, abra-o novamente.</span><span class="sxs-lookup"><span data-stu-id="61791-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="61791-177">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="61791-177">Click **Publish**.</span></span> <span data-ttu-id="61791-178">(Para um site de produção real, copie *app\_offline.htm* para produção e put-lo na pasta do projeto antes da publicação, em seguida, removê-lo quando a implantação for concluída.)</span><span class="sxs-lookup"><span data-stu-id="61791-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="61791-179">Visual Studio implanta as alterações de código para o ambiente de teste e abre o navegador para a home page do Contoso University.</span><span class="sxs-lookup"><span data-stu-id="61791-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="61791-180">Selecione a página instrutores.</span><span class="sxs-lookup"><span data-stu-id="61791-180">Select the Instructors page.</span></span>

<span data-ttu-id="61791-181">Migrações do Code First atualiza o banco de dados da mesma maneira que no ambiente de teste.</span><span class="sxs-lookup"><span data-stu-id="61791-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="61791-182">Você ver a nova coluna de horas de trabalho com os dados propagados.</span><span class="sxs-lookup"><span data-stu-id="61791-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="61791-184">Você tem agora implantado com êxito uma atualização de aplicativo que incluía uma alteração de banco de dados, usando um banco de dados do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="61791-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="61791-185">Mais informações</span><span class="sxs-lookup"><span data-stu-id="61791-185">More Information</span></span>

<span data-ttu-id="61791-186">Isso conclui esta série de tutoriais sobre como implantar um aplicativo da web ASP.NET em um provedor de hospedagem de terceiros.</span><span class="sxs-lookup"><span data-stu-id="61791-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="61791-187">Para obter mais informações sobre qualquer um dos tópicos abordados nesses tutoriais, consulte o [mapa de conteúdo de implantação do ASP.NET](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) no site do MSDN.</span><span class="sxs-lookup"><span data-stu-id="61791-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="61791-188">Confirmações</span><span class="sxs-lookup"><span data-stu-id="61791-188">Acknowledgements</span></span>

<span data-ttu-id="61791-189">Eu gostaria de agradecer a pessoas que fizeram contribuições significativas para o conteúdo desta série de tutoriais:</span><span class="sxs-lookup"><span data-stu-id="61791-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="61791-190">Alberto Poblacion, MVP &amp; MCT, Espanha</span><span class="sxs-lookup"><span data-stu-id="61791-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="61791-191">Jarod Ferguson, MVP de desenvolvimento de plataforma de dados, dos Estados Unidos</span><span class="sxs-lookup"><span data-stu-id="61791-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="61791-192">Harsh Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="61791-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="61791-193">Kristina Olson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="61791-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="61791-194">Mike Papa, Microsoft</span><span class="sxs-lookup"><span data-stu-id="61791-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="61791-195">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="61791-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="61791-196">Raffaele Rialdi, Itália</span><span class="sxs-lookup"><span data-stu-id="61791-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="61791-197">Rick Anderson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="61791-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="61791-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="61791-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="61791-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="61791-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="61791-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="61791-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="61791-201">Srđan Božović, Sérvia</span><span class="sxs-lookup"><span data-stu-id="61791-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="61791-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="61791-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="61791-203">[Anterior](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="61791-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>

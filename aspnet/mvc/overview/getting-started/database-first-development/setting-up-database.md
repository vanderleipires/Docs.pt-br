---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Introdução ao Entity Framework 6 Database First usando MVC 5 | Microsoft Docs
author: tfitzmac
description: Usando o MVC, Entity Framework e o Scaffolding do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Esta série de tutoriais...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: 98deeb91dc2b9a1bad535be1bf1e2ec85dfe4028
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371707"
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a><span data-ttu-id="eae9b-104">Introdução ao Entity Framework 6 Database First usando MVC 5</span><span class="sxs-lookup"><span data-stu-id="eae9b-104">Getting Started with Entity Framework 6 Database First using MVC 5</span></span>
====================
<span data-ttu-id="eae9b-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="eae9b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="eae9b-106">Usando o MVC, Entity Framework e o Scaffolding do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="eae9b-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="eae9b-107">Esta série de tutoriais mostra como automaticamente gerar um código que permite aos usuários exibir, editar, criar e excluir dados que residem em uma tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="eae9b-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="eae9b-108">O código gerado corresponde às colunas na tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="eae9b-108">The generated code corresponds to the columns in the database table.</span></span> <span data-ttu-id="eae9b-109">A última parte da série, você implantará o site e o banco de dados no Azure.</span><span class="sxs-lookup"><span data-stu-id="eae9b-109">In the last part of the series, you will deploy the site and database to Azure.</span></span>
> 
> <span data-ttu-id="eae9b-110">Esta parte da série se concentra na criação de um banco de dados e populá-lo com dados.</span><span class="sxs-lookup"><span data-stu-id="eae9b-110">This part of the series focuses on creating a database and populating it with data.</span></span>
> 
> <span data-ttu-id="eae9b-111">Esta série foi gravada com contribuições de Tom Dykstra e Rick Anderson.</span><span class="sxs-lookup"><span data-stu-id="eae9b-111">This series was written with contributions from Tom Dykstra and Rick Anderson.</span></span> <span data-ttu-id="eae9b-112">Ele foi aprimorado com base nos comentários dos usuários na seção de comentários.</span><span class="sxs-lookup"><span data-stu-id="eae9b-112">It was improved based on feedback from users in the comments section.</span></span>


## <a name="introduction"></a><span data-ttu-id="eae9b-113">Introdução</span><span class="sxs-lookup"><span data-stu-id="eae9b-113">Introduction</span></span>

<span data-ttu-id="eae9b-114">Este tópico mostra como começar com um existente de banco de dados e criar rapidamente um aplicativo web que permite aos usuários interagir com os dados.</span><span class="sxs-lookup"><span data-stu-id="eae9b-114">This topic shows how to start with an existing database and quickly create a web application that enables users to interact with the data.</span></span> <span data-ttu-id="eae9b-115">Ele usa o Entity Framework 6 e ao MVC 5 para compilar o aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="eae9b-115">It uses the Entity Framework 6 and MVC 5 to build the web application.</span></span> <span data-ttu-id="eae9b-116">O recurso de Scaffolding do ASP.NET permite que você gerar automaticamente o código para exibir, atualizar, criar e excluir dados.</span><span class="sxs-lookup"><span data-stu-id="eae9b-116">The ASP.NET Scaffolding feature enables you to automatically generate code for displaying, updating, creating and deleting data.</span></span> <span data-ttu-id="eae9b-117">Usando as ferramentas de publicação no Visual Studio, você pode facilmente implantar o site e o banco de dados no Azure.</span><span class="sxs-lookup"><span data-stu-id="eae9b-117">Using the publishing tools within Visual Studio, you can easily deploy the site and database to Azure.</span></span>

<span data-ttu-id="eae9b-118">Este tópico aborda a situação em que você tenha um banco de dados e deseja gerar código para um aplicativo web com base nos campos do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="eae9b-118">This topic addresses the situation where you have a database and want to generate code for a web application based on the fields of that database.</span></span> <span data-ttu-id="eae9b-119">Essa abordagem é chamada de desenvolvimento de banco de dados primeiro.</span><span class="sxs-lookup"><span data-stu-id="eae9b-119">This approach is called Database First development.</span></span> <span data-ttu-id="eae9b-120">Se você ainda não tiver um banco de dados existente, você pode usar uma abordagem denominada desenvolvimento Code First que envolve a definição de classes de dados e gerar o banco de dados das propriedades de classe.</span><span class="sxs-lookup"><span data-stu-id="eae9b-120">If you do not already have an existing database, you can instead use an approach called Code First development which involves defining data classes and generating the database from the class properties.</span></span>

<span data-ttu-id="eae9b-121">Para obter um exemplo introdutório de desenvolvimento Code First, consulte [Introdução ao ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="eae9b-121">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span> <span data-ttu-id="eae9b-122">Para obter um exemplo mais avançado, consulte [criando um modelo de dados do Entity Framework para um aplicativo do ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="eae9b-122">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="eae9b-123">Para obter orientação sobre como selecionar qual abordagem do Entity Framework para usar, consulte [abordagens de desenvolvimento do Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span><span class="sxs-lookup"><span data-stu-id="eae9b-123">For guidance on selecting which Entity Framework approach to use, see [Entity Framework Development Approaches](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eae9b-124">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="eae9b-124">Prerequisites</span></span>

<span data-ttu-id="eae9b-125">Visual Studio 2013 ou Visual Studio Express 2013 para Web</span><span class="sxs-lookup"><span data-stu-id="eae9b-125">Visual Studio 2013 or Visual Studio Express 2013 for Web</span></span>

## <a name="set-up-the-database"></a><span data-ttu-id="eae9b-126">Configurar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="eae9b-126">Set up the database</span></span>

<span data-ttu-id="eae9b-127">Para imitar o ambiente de ter um banco de dados existente, você primeiro crie um banco de dados com alguns dados previamente preenchidos e, em seguida, crie seu aplicativo web que se conecta ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="eae9b-127">To mimic the environment of having an existing database, you will first create a database with some pre-filled data, and then create your web application that connects to the database.</span></span>

<span data-ttu-id="eae9b-128">Este tutorial foi desenvolvido usando LocalDB com o Visual Studio 2013 ou Visual Studio Express 2013 para Web.</span><span class="sxs-lookup"><span data-stu-id="eae9b-128">This tutorial was developed using LocalDB with either Visual Studio 2013 or Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="eae9b-129">Você pode usar um servidor de banco de dados existente em vez do LocalDB, mas dependendo da sua versão do Visual Studio e seu tipo de banco de dados, todas as ferramentas de dados no Visual Studio não podem ter suporte.</span><span class="sxs-lookup"><span data-stu-id="eae9b-129">You can use an existing database server instead of LocalDB, but depending on your version of Visual Studio and your type of database, all of the data tools in Visual Studio might not be supported.</span></span> <span data-ttu-id="eae9b-130">Se as ferramentas não estão disponíveis para seu banco de dados, você precisa executar algumas etapas específicas do banco de dados dentro do pacote de gerenciamento do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="eae9b-130">If the tools are not available for your database, you may need to perform some of the database-specific steps within the management suite for your database.</span></span>

<span data-ttu-id="eae9b-131">Se você tiver um problema com as ferramentas de banco de dados na sua versão do Visual Studio, verifique se que você instalou a versão mais recente das ferramentas de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="eae9b-131">If you have a problem with the database tools in your version of Visual Studio, make sure you have installed the latest version of the database tools.</span></span> <span data-ttu-id="eae9b-132">Para obter informações sobre como atualizar ou instalar as ferramentas de banco de dados, consulte [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span><span class="sxs-lookup"><span data-stu-id="eae9b-132">For information about updating or installing the database tools, see [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).</span></span>

<span data-ttu-id="eae9b-133">Inicie o Visual Studio e crie uma **projeto de banco de dados do SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="eae9b-133">Launch Visual Studio and create a **SQL Server Database Project**.</span></span> <span data-ttu-id="eae9b-134">Nomeie o projeto **ContosoUniversityData**.</span><span class="sxs-lookup"><span data-stu-id="eae9b-134">Name the project **ContosoUniversityData**.</span></span>

![Criar projeto de banco de dados](setting-up-database/_static/image1.png)

<span data-ttu-id="eae9b-136">Agora você tem um projeto de banco de dados vazio.</span><span class="sxs-lookup"><span data-stu-id="eae9b-136">You now have an empty database project.</span></span> <span data-ttu-id="eae9b-137">Você implantará este banco de dados para o Azure posteriormente no tutorial, portanto, você precisará definir o banco de dados SQL como a plataforma de destino para o projeto.</span><span class="sxs-lookup"><span data-stu-id="eae9b-137">You will deploy this database to Azure later in this tutorial, so you'll need to set Azure SQL Database as the target platform for the project.</span></span> <span data-ttu-id="eae9b-138">Definir a plataforma de destino não realmente implantar o banco de dados; Isso significa apenas que o projeto de banco de dados irá verificar se o design de banco de dados é compatível com a plataforma de destino.</span><span class="sxs-lookup"><span data-stu-id="eae9b-138">Setting the target platform does not actually deploy the database; it only means that the database project will verify that the database design is compatible with the target platform.</span></span> <span data-ttu-id="eae9b-139">Para definir a plataforma de destino, abra o **propriedades** para o projeto e selecione **banco de dados SQL do Microsoft Azure** para a plataforma de destino.</span><span class="sxs-lookup"><span data-stu-id="eae9b-139">To set the target platform, open the **Properties** for the project and select **Microsoft Azure SQL Database** for the target platform.</span></span>

![Defina a plataforma de destino](setting-up-database/_static/image2.png)

<span data-ttu-id="eae9b-141">Você pode criar as tabelas necessárias para este tutorial, adicionando scripts SQL que definem as tabelas.</span><span class="sxs-lookup"><span data-stu-id="eae9b-141">You can create the tables needed for this tutorial by adding SQL scripts that define the tables.</span></span> <span data-ttu-id="eae9b-142">Clique em seu projeto e adicionar um novo item.</span><span class="sxs-lookup"><span data-stu-id="eae9b-142">Right-click your project and add a new item.</span></span>

![Adicionar novo item](setting-up-database/_static/image3.png)

<span data-ttu-id="eae9b-144">Adicione uma nova tabela nomeada aluno.</span><span class="sxs-lookup"><span data-stu-id="eae9b-144">Add a new table named Student.</span></span>

![Adicionar tabela aluno](setting-up-database/_static/image4.png)

<span data-ttu-id="eae9b-146">No arquivo de tabela, substitua o comando T-SQL com o código a seguir para criar a tabela.</span><span class="sxs-lookup"><span data-stu-id="eae9b-146">In the table file, replace the T-SQL command with the following code to create the table.</span></span>

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

<span data-ttu-id="eae9b-147">Observe que a janela de design sincroniza automaticamente com o código.</span><span class="sxs-lookup"><span data-stu-id="eae9b-147">Notice that the design window automatically synchronizes with the code.</span></span> <span data-ttu-id="eae9b-148">Você pode trabalhar com o código ou o designer.</span><span class="sxs-lookup"><span data-stu-id="eae9b-148">You can work with either the code or designer.</span></span>

![Mostrar código e design](setting-up-database/_static/image5.png)

<span data-ttu-id="eae9b-150">Adicione outra tabela.</span><span class="sxs-lookup"><span data-stu-id="eae9b-150">Add another table.</span></span> <span data-ttu-id="eae9b-151">Desta vez Nomeie-o curso e use o seguinte comando T-SQL.</span><span class="sxs-lookup"><span data-stu-id="eae9b-151">This time name it Course and use the following T-SQL command.</span></span>

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

<span data-ttu-id="eae9b-152">E, então, repetir mais uma vez para criar uma tabela chamada de registro.</span><span class="sxs-lookup"><span data-stu-id="eae9b-152">And, repeat one more time to create a table named Enrollment.</span></span>

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

<span data-ttu-id="eae9b-153">Você pode preencher seu banco de dados com dados por meio de um script que é executado depois que o banco de dados é implantado.</span><span class="sxs-lookup"><span data-stu-id="eae9b-153">You can populate your database with data through a script that is run after the database is deployed.</span></span> <span data-ttu-id="eae9b-154">Adicione um Script de pós-implantação ao projeto.</span><span class="sxs-lookup"><span data-stu-id="eae9b-154">Add a Post-Deployment Script to the project.</span></span> <span data-ttu-id="eae9b-155">Você pode usar o nome padrão.</span><span class="sxs-lookup"><span data-stu-id="eae9b-155">You can use the default name.</span></span>

![adicionar script pós-implantação](setting-up-database/_static/image6.png)

<span data-ttu-id="eae9b-157">Adicione o seguinte código T-SQL para o script de pós-implantação.</span><span class="sxs-lookup"><span data-stu-id="eae9b-157">Add the following T-SQL code to the post-deployment script.</span></span> <span data-ttu-id="eae9b-158">Esse script simplesmente adiciona dados ao banco de dados quando nenhum registro correspondente for encontrado.</span><span class="sxs-lookup"><span data-stu-id="eae9b-158">This script simply adds data to the database when no matching record is found.</span></span> <span data-ttu-id="eae9b-159">Não substituir ou excluir quaisquer dados que talvez você tenha inserido no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="eae9b-159">It does not overwrite or delete any data you may have entered into the database.</span></span>

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

<span data-ttu-id="eae9b-160">É importante observar que o script de pós-implantação é executado sempre que você implanta seu projeto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="eae9b-160">It is important to note that the post-deployment script is run every time you deploy your database project.</span></span> <span data-ttu-id="eae9b-161">Portanto, você precisa considerar cuidadosamente suas necessidades ao escrever este script.</span><span class="sxs-lookup"><span data-stu-id="eae9b-161">Therefore, you need to carefully consider your requirements when writing this script.</span></span> <span data-ttu-id="eae9b-162">Em alguns casos, talvez seja conveniente iniciar a partir um conjunto conhecido de dados toda vez que o projeto é implantado.</span><span class="sxs-lookup"><span data-stu-id="eae9b-162">In some cases, you may wish to start over from a known set of data every time the project is deployed.</span></span> <span data-ttu-id="eae9b-163">Em outros casos, talvez não queira alterar os dados existentes de qualquer forma.</span><span class="sxs-lookup"><span data-stu-id="eae9b-163">In other cases, you may not want to alter the existing data in any way.</span></span> <span data-ttu-id="eae9b-164">De acordo com suas necessidades, você pode decidir se é necessário um script de pós-implantação ou o que você precisa incluir no script.</span><span class="sxs-lookup"><span data-stu-id="eae9b-164">Based on your requirements, you can decide whether you need a post-deployment script or what you need to include in the script.</span></span> <span data-ttu-id="eae9b-165">Para obter mais informações sobre a população de seu banco de dados com um script de pós-implantação, consulte [incluindo dados em um projeto de banco de dados do SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span><span class="sxs-lookup"><span data-stu-id="eae9b-165">For more information about populating your database with a post-deployment script, see [Including Data in a SQL Server Database Project](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).</span></span>

<span data-ttu-id="eae9b-166">Agora você tem arquivos de script SQL 4, mas nenhuma tabela real.</span><span class="sxs-lookup"><span data-stu-id="eae9b-166">You now have 4 SQL script files but no actual tables.</span></span> <span data-ttu-id="eae9b-167">Você está pronto para implantar seu projeto de banco de dados localdb.</span><span class="sxs-lookup"><span data-stu-id="eae9b-167">You are ready to deploy your database project to localdb.</span></span> <span data-ttu-id="eae9b-168">No Visual Studio, clique no botão Iniciar (ou F5) para compilar e implantar seu projeto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="eae9b-168">In Visual Studio, click the Start button (or F5) to build and deploy your database project.</span></span> <span data-ttu-id="eae9b-169">Verifique a guia de saída para verificar se a compilação e implantação foi bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="eae9b-169">Check the Output tab to verify that the build and deployment succeeded.</span></span>

![Mostrar saída](setting-up-database/_static/image7.png)

<span data-ttu-id="eae9b-171">Para ver que o novo banco de dados foi criado, abra **Pesquisador de objetos do SQL Server** e procure o nome do projeto no servidor de banco de dados local correto (nesse caso **\ProjectsV12 (localdb)**).</span><span class="sxs-lookup"><span data-stu-id="eae9b-171">To see that the new database has been created, open **SQL Server Object Explorer** and look for the name of the project in the correct local database server (in this case **(localdb)\ProjectsV12**).</span></span>

![Mostrar o novo banco de dados](setting-up-database/_static/image8.png)

<span data-ttu-id="eae9b-173">Para ver que as tabelas são populadas com dados, uma tabela com o botão direito e selecione **exibir dados**.</span><span class="sxs-lookup"><span data-stu-id="eae9b-173">To see that the tables are populated with data, right-click a table, and select **View Data**.</span></span>

![Mostrar dados da tabela](setting-up-database/_static/image9.png)

<span data-ttu-id="eae9b-175">Uma exibição editável dos dados da tabela é exibida.</span><span class="sxs-lookup"><span data-stu-id="eae9b-175">An editable view of the table data is displayed.</span></span>

![Mostrar resultados de dados de tabela](setting-up-database/_static/image10.png)

<span data-ttu-id="eae9b-177">Seu banco de dados agora está configurado e preenchido com dados.</span><span class="sxs-lookup"><span data-stu-id="eae9b-177">Your database is now set up and populated with data.</span></span> <span data-ttu-id="eae9b-178">O próximo tutorial, você criará um aplicativo web para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="eae9b-178">In the next tutorial, you will create a web application for the database.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="eae9b-179">Avançar</span><span class="sxs-lookup"><span data-stu-id="eae9b-179">Next</span></span>](creating-the-web-application.md)

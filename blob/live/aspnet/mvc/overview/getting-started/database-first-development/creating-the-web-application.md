---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Banco de dados EF primeiro com o ASP.NET MVC: Criando o aplicativo Web e modelos de dados | Microsoft Docs'
author: tfitzmac
description: "Usando o MVC, Entity Framework e estrutura do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente. Este tutorial série..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: f495bfa3aa5332e4ca3e44c2ffbfb760fbbeafc8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a><span data-ttu-id="a18f7-104">Banco de dados EF primeiro com o ASP.NET MVC: Criando o aplicativo Web e modelos de dados</span><span class="sxs-lookup"><span data-stu-id="a18f7-104">EF Database First with ASP.NET MVC: Creating the Web Application and Data Models</span></span>
====================
<span data-ttu-id="a18f7-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a18f7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a18f7-106">Usando o MVC, Entity Framework e estrutura do ASP.NET, você pode criar um aplicativo web que fornece uma interface para um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="a18f7-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="a18f7-107">Esta série de tutorial mostra como automaticamente gerar um código que permite aos usuários exibir, editar, criar e excluir dados que residem em uma tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a18f7-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="a18f7-108">O código gerado corresponde às colunas na tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a18f7-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="a18f7-109">Esta parte da série se concentra em criar o aplicativo web e gerar os modelos de dados com base em suas tabelas de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a18f7-109">This part of the series focuses on creating the web application, and generating the data models based on your database tables.</span></span>


## <a name="create-a-new-aspnet-web-application"></a><span data-ttu-id="a18f7-110">Criar um novo aplicativo Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a18f7-110">Create a new ASP.NET Web Application</span></span>

<span data-ttu-id="a18f7-111">Em uma nova solução ou a mesma solução que o projeto de banco de dados, crie um novo projeto no Visual Studio e selecione o **aplicativo Web ASP.NET** modelo.</span><span class="sxs-lookup"><span data-stu-id="a18f7-111">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="a18f7-112">Nomeie o projeto **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="a18f7-112">Name the project **ContosoSite**.</span></span>

![Criar projeto](creating-the-web-application/_static/image1.png)

<span data-ttu-id="a18f7-114">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="a18f7-114">Click **OK**.</span></span>

<span data-ttu-id="a18f7-115">Na janela do novo projeto ASP.NET, selecione o **MVC** modelo.</span><span class="sxs-lookup"><span data-stu-id="a18f7-115">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="a18f7-116">Você pode limpar o **Host na nuvem** opção agora porque você implantará o aplicativo para a nuvem.</span><span class="sxs-lookup"><span data-stu-id="a18f7-116">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="a18f7-117">Clique em **Okey** para criar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a18f7-117">Click **OK** to create the application.</span></span>

![Selecione o modelo mvc](creating-the-web-application/_static/image2.png)

<span data-ttu-id="a18f7-119">O projeto é criado com o padrão de arquivos e pastas.</span><span class="sxs-lookup"><span data-stu-id="a18f7-119">The project is created with the default files and folders.</span></span>

<span data-ttu-id="a18f7-120">Neste tutorial, você usará o Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="a18f7-120">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="a18f7-121">Você pode verificar a versão do Entity Framework em seu projeto usando a janela Gerenciar pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="a18f7-121">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="a18f7-122">Se necessário, atualize sua versão do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a18f7-122">If necessary, update your version of Entity Framework.</span></span>

![Mostrar versão](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="a18f7-124">Gerar os modelos</span><span class="sxs-lookup"><span data-stu-id="a18f7-124">Generate the models</span></span>

<span data-ttu-id="a18f7-125">Agora você criará modelos Entity Framework das tabelas do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a18f7-125">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="a18f7-126">Esses modelos são classes que você usará para trabalhar com os dados.</span><span class="sxs-lookup"><span data-stu-id="a18f7-126">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="a18f7-127">Cada modelo reflete uma tabela no banco de dados e contém propriedades que correspondem às colunas na tabela.</span><span class="sxs-lookup"><span data-stu-id="a18f7-127">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="a18f7-128">Clique com botão direito do **modelos** pasta e selecione **adicionar** e **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="a18f7-128">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

![Adicionar novo item](creating-the-web-application/_static/image4.png)

<span data-ttu-id="a18f7-130">Na janela Adicionar Novo Item, selecione **dados** no painel esquerdo e **modelo de dados de entidade ADO.NET** nas opções do painel central.</span><span class="sxs-lookup"><span data-stu-id="a18f7-130">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="a18f7-131">Nomeie o novo arquivo de modelo **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="a18f7-131">Name the new model file **ContosoModel**.</span></span>

![Criar modelo](creating-the-web-application/_static/image5.png)

<span data-ttu-id="a18f7-133">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="a18f7-133">Click **Add**.</span></span>

<span data-ttu-id="a18f7-134">No Assistente de modelo de dados de entidade, selecione **EF Designer do banco de dados**.</span><span class="sxs-lookup"><span data-stu-id="a18f7-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

![Gerar do banco de dados](creating-the-web-application/_static/image6.png)

<span data-ttu-id="a18f7-136">Clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="a18f7-136">Click **Next**.</span></span>

<span data-ttu-id="a18f7-137">Se você tiver conexões de banco de dados definidas dentro de seu ambiente de desenvolvimento, você poderá ver uma dessas conexões previamente selecionadas.</span><span class="sxs-lookup"><span data-stu-id="a18f7-137">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="a18f7-138">No entanto, você deseja criar uma nova conexão para o banco de dados que você criou na primeira parte deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="a18f7-138">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="a18f7-139">Clique o **nova Conexão** botão.</span><span class="sxs-lookup"><span data-stu-id="a18f7-139">Click the **New Connection** button.</span></span>

![Conecte-se ao banco de dados](creating-the-web-application/_static/image7.png)

<span data-ttu-id="a18f7-141">Na janela Propriedades de Conexão, forneça o nome do servidor local onde seu banco de dados foi criado (neste caso **\ProjectsV12 (localdb)**).</span><span class="sxs-lookup"><span data-stu-id="a18f7-141">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV12**).</span></span> <span data-ttu-id="a18f7-142">Depois de fornecer o nome do servidor, selecione o ContosoUniversityData de bancos de dados disponíveis.</span><span class="sxs-lookup"><span data-stu-id="a18f7-142">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![definir propriedades de conexão](creating-the-web-application/_static/image8.png)

<span data-ttu-id="a18f7-144">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="a18f7-144">Click **OK**.</span></span>

<span data-ttu-id="a18f7-145">As propriedades de conexão corretas são exibidas agora.</span><span class="sxs-lookup"><span data-stu-id="a18f7-145">The correct connection properties are now displayed.</span></span> <span data-ttu-id="a18f7-146">Você pode usar o nome padrão para a conexão no arquivo Web. config</span><span class="sxs-lookup"><span data-stu-id="a18f7-146">You can use the default name for connection in the Web.Config file</span></span>

![configurações de conexão](creating-the-web-application/_static/image9.png)

<span data-ttu-id="a18f7-148">Clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="a18f7-148">Click **Next**.</span></span>

<span data-ttu-id="a18f7-149">Selecione **tabelas** para gerar modelos para todas as três tabelas.</span><span class="sxs-lookup"><span data-stu-id="a18f7-149">Select **Tables** to generate models for all three tables.</span></span>

![Selecionar tabelas](creating-the-web-application/_static/image10.png)

<span data-ttu-id="a18f7-151">Clique em **Finalizar**.</span><span class="sxs-lookup"><span data-stu-id="a18f7-151">Click **Finish**.</span></span>

<span data-ttu-id="a18f7-152">Se você receber um aviso de segurança, selecione **Okey** para continuar a execução do modelo.</span><span class="sxs-lookup"><span data-stu-id="a18f7-152">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="a18f7-153">Os modelos são gerados a partir de tabelas de banco de dados e um exibição do diagrama que mostra as propriedades e as relações entre as tabelas.</span><span class="sxs-lookup"><span data-stu-id="a18f7-153">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![diagrama do modelo](creating-the-web-application/_static/image11.png)

<span data-ttu-id="a18f7-155">A pasta de modelos agora inclui muitos novos arquivos relacionados a modelos que foram gerados do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a18f7-155">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

![Mostrar os novos arquivos de modelo](creating-the-web-application/_static/image12.png)

<span data-ttu-id="a18f7-157">O **ContosoModel.Context.cs** arquivo contém uma classe que deriva de **DbContext** classe e fornece uma propriedade para cada classe de modelo que corresponde a uma tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a18f7-157">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="a18f7-158">O **Course.cs**, **Enrollment.cs**, e **Student.cs** arquivos contêm as classes de modelo que representam as tabelas de bancos de dados.</span><span class="sxs-lookup"><span data-stu-id="a18f7-158">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="a18f7-159">Você usará a classe de contexto e as classes de modelo ao trabalhar com scaffolding.</span><span class="sxs-lookup"><span data-stu-id="a18f7-159">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="a18f7-160">Antes de continuar com este tutorial, crie o projeto.</span><span class="sxs-lookup"><span data-stu-id="a18f7-160">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="a18f7-161">Na próxima seção, você irá gerar o código com base nos modelos de dados, mas essa seção não funcionará se o projeto não foi criado.</span><span class="sxs-lookup"><span data-stu-id="a18f7-161">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a18f7-162">[Anterior](setting-up-database.md)
[Próximo](generating-views.md)</span><span class="sxs-lookup"><span data-stu-id="a18f7-162">[Previous](setting-up-database.md)
[Next](generating-views.md)</span></span>

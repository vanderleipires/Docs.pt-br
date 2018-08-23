---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Exibindo uma tabela de banco de dados (c#) | Microsoft Docs
author: microsoft
description: Neste tutorial, demonstrarei dois métodos de exibição de um conjunto de registros do banco de dados. Eu mostro dois métodos de formatação de um conjunto de registros de banco de dados em um HTML ta...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: d0d3f6a574a4b923d5da73ccb2ab3bfbd6f305ef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825203"
---
<a name="displaying-a-table-of-database-data-c"></a><span data-ttu-id="f6407-104">Exibindo uma tabela de banco de dados (c#)</span><span class="sxs-lookup"><span data-stu-id="f6407-104">Displaying a Table of Database Data (C#)</span></span>
====================
<span data-ttu-id="f6407-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f6407-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="f6407-106">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="f6407-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> <span data-ttu-id="f6407-107">Neste tutorial, demonstrarei dois métodos de exibição de um conjunto de registros do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f6407-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="f6407-108">Eu mostro dois métodos de formatação de um conjunto de registros de banco de dados em uma tabela HTML.</span><span class="sxs-lookup"><span data-stu-id="f6407-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="f6407-109">Primeiro, vou mostrar como você pode formatar os registros de banco de dados diretamente dentro de um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="f6407-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="f6407-110">Em seguida, demonstrarei como você pode tirar proveito de parciais ao formatar os registros de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f6407-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="f6407-111">O objetivo deste tutorial é explicar como você pode exibir uma tabela HTML de dados do banco de dados em um aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f6407-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="f6407-112">Primeiro, você aprenderá a usar as ferramentas de scaffolding incluídas no Visual Studio para gerar uma exibição que exibe um conjunto de registros automaticamente.</span><span class="sxs-lookup"><span data-stu-id="f6407-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="f6407-113">Em seguida, você aprenderá a usar um parcial como um modelo ao formatar os registros de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f6407-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="f6407-114">Criar as Classes de modelo</span><span class="sxs-lookup"><span data-stu-id="f6407-114">Create the Model Classes</span></span>

<span data-ttu-id="f6407-115">Vamos para exibir o conjunto de registros da tabela de banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="f6407-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="f6407-116">A tabela de banco de dados de filmes contém as seguintes colunas:</span><span class="sxs-lookup"><span data-stu-id="f6407-116">The Movies database table contains the following columns:</span></span>

<a id="0.3_table01"></a>


| <span data-ttu-id="f6407-117">**Nome da coluna**</span><span class="sxs-lookup"><span data-stu-id="f6407-117">**Column Name**</span></span> | <span data-ttu-id="f6407-118">**Tipo de dados**</span><span class="sxs-lookup"><span data-stu-id="f6407-118">**Data Type**</span></span> | <span data-ttu-id="f6407-119">**Permitir nulos**</span><span class="sxs-lookup"><span data-stu-id="f6407-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f6407-120">Id</span><span class="sxs-lookup"><span data-stu-id="f6407-120">Id</span></span> | <span data-ttu-id="f6407-121">int</span><span class="sxs-lookup"><span data-stu-id="f6407-121">Int</span></span> | <span data-ttu-id="f6407-122">False</span><span class="sxs-lookup"><span data-stu-id="f6407-122">False</span></span> |
| <span data-ttu-id="f6407-123">Título</span><span class="sxs-lookup"><span data-stu-id="f6407-123">Title</span></span> | <span data-ttu-id="f6407-124">Nvarchar(200)</span><span class="sxs-lookup"><span data-stu-id="f6407-124">Nvarchar(200)</span></span> | <span data-ttu-id="f6407-125">False</span><span class="sxs-lookup"><span data-stu-id="f6407-125">False</span></span> |
| <span data-ttu-id="f6407-126">Diretor</span><span class="sxs-lookup"><span data-stu-id="f6407-126">Director</span></span> | <span data-ttu-id="f6407-127">Nvarchar (50)</span><span class="sxs-lookup"><span data-stu-id="f6407-127">NVarchar(50)</span></span> | <span data-ttu-id="f6407-128">False</span><span class="sxs-lookup"><span data-stu-id="f6407-128">False</span></span> |
| <span data-ttu-id="f6407-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="f6407-129">DateReleased</span></span> | <span data-ttu-id="f6407-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="f6407-130">DateTime</span></span> | <span data-ttu-id="f6407-131">False</span><span class="sxs-lookup"><span data-stu-id="f6407-131">False</span></span> |


<span data-ttu-id="f6407-132">Para representar a tabela de filmes em nosso aplicativo ASP.NET MVC, precisamos criar uma classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="f6407-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="f6407-133">Neste tutorial, usamos o Microsoft Entity Framework para criar nossas classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="f6407-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="f6407-134">Neste tutorial, usamos o Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f6407-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="f6407-135">No entanto, é importante entender que você pode usar uma variedade de tecnologias diferentes para interagir com um banco de dados de um aplicativo ASP.NET MVC incluindo LINQ to SQL, NHibernate ou ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="f6407-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="f6407-136">Siga estas etapas para iniciar o Assistente de modelo de dados de entidade:</span><span class="sxs-lookup"><span data-stu-id="f6407-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="f6407-137">Clique com botão direito na pasta de modelos na janela do Gerenciador de soluções e selecione a opção de menu **adicionar, Item novo**.</span><span class="sxs-lookup"><span data-stu-id="f6407-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="f6407-138">Selecione o **dados** categoria e selecione o **modelo de dados de entidade ADO.NET** modelo.</span><span class="sxs-lookup"><span data-stu-id="f6407-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="f6407-139">Dê o nome de seu modelo de dados *MoviesDBModel.edmx* e clique no **Add** botão.</span><span class="sxs-lookup"><span data-stu-id="f6407-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="f6407-140">Depois de clicar no botão Add, o Assistente de modelo de dados de entidade é exibida (veja a Figura 1).</span><span class="sxs-lookup"><span data-stu-id="f6407-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="f6407-141">Siga estas etapas para concluir o Assistente:</span><span class="sxs-lookup"><span data-stu-id="f6407-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="f6407-142">No **escolher conteúdo do modelo** etapa, selecione o **gerar a partir do banco de dados** opção.</span><span class="sxs-lookup"><span data-stu-id="f6407-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="f6407-143">No **escolha sua Conexão de dados** etapa, use o *MoviesDB.mdf* conexão de dados e o nome *MoviesDBEntities* para as configurações de conexão.</span><span class="sxs-lookup"><span data-stu-id="f6407-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="f6407-144">Clique o **próxima** botão.</span><span class="sxs-lookup"><span data-stu-id="f6407-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="f6407-145">No **Choose Your Database Objects** etapa, expanda o nó Tables, selecione a tabela de filmes.</span><span class="sxs-lookup"><span data-stu-id="f6407-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="f6407-146">Insira o namespace *modelos* e clique no **concluir** botão.</span><span class="sxs-lookup"><span data-stu-id="f6407-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="f6407-147">[![Criando o LINQ para classes SQL](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f6407-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)</span></span>

<span data-ttu-id="f6407-148">**Figura 01**: Criando classes LINQ to SQL ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="f6407-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image2.png))</span></span>


<span data-ttu-id="f6407-149">Depois de concluir o Assistente de modelo de dados de entidade, o Designer de modelo de dados de entidade é aberto.</span><span class="sxs-lookup"><span data-stu-id="f6407-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="f6407-150">O Designer deve exibir a entidade de filmes (veja a Figura 2).</span><span class="sxs-lookup"><span data-stu-id="f6407-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="f6407-151">[![O Designer de modelo de dados de entidade](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f6407-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)</span></span>

<span data-ttu-id="f6407-152">**Figura 02**: O Designer de modelo de dados de entidade ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="f6407-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image4.png))</span></span>


<span data-ttu-id="f6407-153">Precisamos fazer uma alteração antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="f6407-153">We need to make one change before we continue.</span></span> <span data-ttu-id="f6407-154">O Assistente de dados de entidade gera uma classe de modelo chamada *filmes* que representa a tabela de banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="f6407-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="f6407-155">Como vamos usar a classe de filmes para representar um filme específico, precisamos modificar o nome da classe para ser *filme* em vez de *filmes* (singular em vez de no plural).</span><span class="sxs-lookup"><span data-stu-id="f6407-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="f6407-156">Duas vezes no nome da classe na superfície do designer e altere o nome da classe de filmes para filme.</span><span class="sxs-lookup"><span data-stu-id="f6407-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="f6407-157">Após fazer essa alteração, clique o **salvar** botão (o ícone de disquete) para gerar a classe de filme.</span><span class="sxs-lookup"><span data-stu-id="f6407-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="f6407-158">Criar o controlador de filmes</span><span class="sxs-lookup"><span data-stu-id="f6407-158">Create the Movies Controller</span></span>

<span data-ttu-id="f6407-159">Agora que temos uma forma de representar nossos registros do banco de dados, podemos criar um controlador que retorna a coleção de filmes.</span><span class="sxs-lookup"><span data-stu-id="f6407-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="f6407-160">Dentro da janela do Gerenciador de soluções do Visual Studio, clique com botão direito na pasta controladores e selecione a opção de menu **Add, controlador** (veja a Figura 3).</span><span class="sxs-lookup"><span data-stu-id="f6407-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="f6407-161">[![O controlador Menu adicionar](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f6407-161">[![The Add Controller Menu](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)</span></span>

<span data-ttu-id="f6407-162">**Figura 03**: O adicionar controlador Menu ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f6407-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image6.png))</span></span>


<span data-ttu-id="f6407-163">Quando o **Adicionar controlador** caixa de diálogo for exibida, insira o nome do controlador MovieController (veja a Figura 4).</span><span class="sxs-lookup"><span data-stu-id="f6407-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="f6407-164">Clique o **adicionar** botão para adicionar o novo controlador.</span><span class="sxs-lookup"><span data-stu-id="f6407-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="f6407-165">[![A caixa de diálogo Adicionar controlador](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f6407-165">[![The Add Controller dialog](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)</span></span>

<span data-ttu-id="f6407-166">**Figura 04**: caixa de diálogo o adicionar controlador ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="f6407-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image8.png))</span></span>


<span data-ttu-id="f6407-167">Precisamos modificar a ação Index () exposta pelo controlador de filmes para que ele retorne o conjunto de registros do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f6407-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="f6407-168">Modificar o controlador para que ele se parece com o controlador na listagem 1.</span><span class="sxs-lookup"><span data-stu-id="f6407-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="f6407-169">**Listagem 1 – Controllers\MovieController.cs**</span><span class="sxs-lookup"><span data-stu-id="f6407-169">**Listing 1 – Controllers\MovieController.cs**</span></span>

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

<span data-ttu-id="f6407-170">Na listagem 1, a classe MoviesDBEntities é usada para representar o banco de dados MoviesDB.</span><span class="sxs-lookup"><span data-stu-id="f6407-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="f6407-171">Para usar essa classe, você precisa importar o namespace MvcApplication1.Models como este:</span><span class="sxs-lookup"><span data-stu-id="f6407-171">To use this class, you need to import the MvcApplication1.Models namespace like this:</span></span>

<span data-ttu-id="f6407-172">usando MvcApplication1.Models;</span><span class="sxs-lookup"><span data-stu-id="f6407-172">using MvcApplication1.Models;</span></span>

<span data-ttu-id="f6407-173">A expressão *entidades. MovieSet.ToList()* retorna o conjunto de todos os filmes da tabela de banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="f6407-173">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="f6407-174">Criar o modo de exibição</span><span class="sxs-lookup"><span data-stu-id="f6407-174">Create the View</span></span>

<span data-ttu-id="f6407-175">A maneira mais fácil para exibir um conjunto de registros do banco de dados em uma tabela HTML é aproveitar o scaffolding fornecido pelo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f6407-175">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="f6407-176">Criar seu aplicativo, selecionando a opção de menu **Build Build Solution**.</span><span class="sxs-lookup"><span data-stu-id="f6407-176">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="f6407-177">Você deve criar seu aplicativo antes de abrir o **adicionar exibição** suas classes de dados ou de caixa de diálogo não aparecerão na caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="f6407-177">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="f6407-178">A ação Index () com o botão direito e selecione a opção de menu **adicionar exibição** (consulte a Figura 5).</span><span class="sxs-lookup"><span data-stu-id="f6407-178">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="f6407-179">[![Adicionando uma exibição](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="f6407-179">[![Adding a view](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)</span></span>

<span data-ttu-id="f6407-180">**Figura 05**: adicionando uma exibição ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="f6407-180">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image10.png))</span></span>


<span data-ttu-id="f6407-181">No **adicionar exibição** caixa de diálogo, marque a caixa de seleção **criar uma exibição fortemente tipada**.</span><span class="sxs-lookup"><span data-stu-id="f6407-181">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="f6407-182">Selecione a classe de filme como a **exibir dados de classe**.</span><span class="sxs-lookup"><span data-stu-id="f6407-182">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="f6407-183">Selecione *lista* como o **exibir conteúdo** (veja a Figura 6).</span><span class="sxs-lookup"><span data-stu-id="f6407-183">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="f6407-184">Selecione essas opções irá gerar uma exibição fortemente tipada que exibe uma lista de filmes.</span><span class="sxs-lookup"><span data-stu-id="f6407-184">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="f6407-185">[![A caixa de diálogo Adicionar modo de exibição](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="f6407-185">[![The Add View dialog](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)</span></span>

<span data-ttu-id="f6407-186">**Figura 06**: caixa de diálogo Adicionar exibição do ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="f6407-186">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image12.png))</span></span>


<span data-ttu-id="f6407-187">Depois de clicar na **adicionar** botão, o modo de exibição na listagem 2 é gerado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="f6407-187">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="f6407-188">Essa exibição contém o código necessário para iterar pela coleção de filmes e exibir cada uma das propriedades de um filme.</span><span class="sxs-lookup"><span data-stu-id="f6407-188">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="f6407-189">**Listagem 2 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="f6407-189">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

<span data-ttu-id="f6407-190">Você pode executar o aplicativo, selecionando a opção de menu **depurar, iniciar depuração** (ou pressionando a tecla F5).</span><span class="sxs-lookup"><span data-stu-id="f6407-190">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="f6407-191">Executando o aplicativo inicia o Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="f6407-191">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="f6407-192">Se você navegar até a URL /Movie, em seguida, você verá a página na Figura 7.</span><span class="sxs-lookup"><span data-stu-id="f6407-192">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="f6407-193">[![Uma tabela de filmes](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="f6407-193">[![A table of movies](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)</span></span>

<span data-ttu-id="f6407-194">**Figura 07**: uma tabela de filmes ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="f6407-194">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-cs/_static/image14.png))</span></span>


<span data-ttu-id="f6407-195">Se você não gostar nada sobre a aparência da grade de registros de banco de dados na Figura 7, em seguida, você pode simplesmente modificar o modo de exibição de índice.</span><span class="sxs-lookup"><span data-stu-id="f6407-195">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="f6407-196">Por exemplo, você pode alterar o *DateReleased* cabeçalho *data de lançamento* modificando a exibição de índice.</span><span class="sxs-lookup"><span data-stu-id="f6407-196">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="f6407-197">Criar um modelo com um parcial</span><span class="sxs-lookup"><span data-stu-id="f6407-197">Create a Template with a Partial</span></span>

<span data-ttu-id="f6407-198">Quando um modo de exibição fica muito complicado, ele é uma boa ideia começar invadir o modo de exibição parciais.</span><span class="sxs-lookup"><span data-stu-id="f6407-198">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="f6407-199">Usar parciais torna mais fácil de entender e manter seus modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="f6407-199">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="f6407-200">Vamos criar um parcial que podemos usar como modelo para formatar cada um dos registros de banco de dados do filme.</span><span class="sxs-lookup"><span data-stu-id="f6407-200">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="f6407-201">Siga estas etapas para criar parcial:</span><span class="sxs-lookup"><span data-stu-id="f6407-201">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="f6407-202">Clique com botão direito na pasta Views\Movie e selecione a opção de menu **adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="f6407-202">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="f6407-203">Marque a caixa de seleção rotulada *criar uma exibição parcial (. ascx)*.</span><span class="sxs-lookup"><span data-stu-id="f6407-203">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="f6407-204">Nome parcial *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="f6407-204">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="f6407-205">Marque a caixa de seleção rotulada **criar uma exibição fortemente tipada**.</span><span class="sxs-lookup"><span data-stu-id="f6407-205">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="f6407-206">Selecione o filme como a *exibir dados de classe*.</span><span class="sxs-lookup"><span data-stu-id="f6407-206">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="f6407-207">Selecione vazio como o *exibir o conteúdo*.</span><span class="sxs-lookup"><span data-stu-id="f6407-207">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="f6407-208">Clique o **adicionar** botão Adicionar parcial ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="f6407-208">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="f6407-209">Depois de concluir essas etapas, modifique o MovieTemplate parcial para se parecer com a listagem 3.</span><span class="sxs-lookup"><span data-stu-id="f6407-209">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="f6407-210">**Listagem 3 – Views\Movie\MovieTemplate.ascx**</span><span class="sxs-lookup"><span data-stu-id="f6407-210">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

<span data-ttu-id="f6407-211">Parcial na listagem 3 contém um modelo para uma única linha de registros.</span><span class="sxs-lookup"><span data-stu-id="f6407-211">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="f6407-212">O modo de exibição do índice modificado na listagem 4 usa o MovieTemplate parcial.</span><span class="sxs-lookup"><span data-stu-id="f6407-212">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="f6407-213">**Listagem 4 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="f6407-213">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

<span data-ttu-id="f6407-214">A exibição na listagem 4 contém um loop foreach que itera em todos os filmes.</span><span class="sxs-lookup"><span data-stu-id="f6407-214">The view in Listing 4 contains a foreach loop that iterates through all of the movies.</span></span> <span data-ttu-id="f6407-215">Para cada filme, o MovieTemplate parcial é usado para formatar o filme.</span><span class="sxs-lookup"><span data-stu-id="f6407-215">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="f6407-216">O MovieTemplate é renderizado chamando o método auxiliar RenderPartial().</span><span class="sxs-lookup"><span data-stu-id="f6407-216">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="f6407-217">O modo de exibição do índice modificado renderiza a mesma tabela HTML de registros de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f6407-217">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="f6407-218">No entanto, o modo de exibição foi bastante simplificado.</span><span class="sxs-lookup"><span data-stu-id="f6407-218">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="f6407-219">O método RenderPartial() é diferente da maioria dos outros métodos auxiliares, porque ele não retorna uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="f6407-219">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="f6407-220">Portanto, você deve chamar o método RenderPartial() usando &lt;% Html.RenderPartial(); %&gt; em vez de &lt;% = Html.RenderPartial(); %&gt;.</span><span class="sxs-lookup"><span data-stu-id="f6407-220">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial(); %&gt; instead of &lt;%= Html.RenderPartial(); %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="f6407-221">Resumo</span><span class="sxs-lookup"><span data-stu-id="f6407-221">Summary</span></span>

<span data-ttu-id="f6407-222">O objetivo deste tutorial era explicar como você pode exibir um conjunto de registros do banco de dados em uma tabela HTML.</span><span class="sxs-lookup"><span data-stu-id="f6407-222">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="f6407-223">Primeiro, você aprendeu como retornar um conjunto de registros do banco de dados de uma ação do controlador, tirando proveito do Microsoft Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f6407-223">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="f6407-224">Em seguida, você aprendeu como usar o scaffolding do Visual Studio para gerar uma exibição que exibe uma coleção de itens automaticamente.</span><span class="sxs-lookup"><span data-stu-id="f6407-224">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="f6407-225">Por fim, você aprendeu a simplificar a exibição, aproveitando um parcial.</span><span class="sxs-lookup"><span data-stu-id="f6407-225">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="f6407-226">Você aprendeu a usar um parcial como um modelo para que você pode formatar cada registro de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f6407-226">You learned how to use a partial as a template so that you can format each database record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f6407-227">[Anterior](creating-model-classes-with-linq-to-sql-cs.md)
> [Próximo](performing-simple-validation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f6407-227">[Previous](creating-model-classes-with-linq-to-sql-cs.md)
[Next](performing-simple-validation-cs.md)</span></span>

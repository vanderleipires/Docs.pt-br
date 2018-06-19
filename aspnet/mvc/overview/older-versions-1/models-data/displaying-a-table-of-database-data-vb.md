---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: Exibindo uma tabela de banco de dados (VB) | Microsoft Docs
author: microsoft
description: Neste tutorial, demonstrarei dois métodos de exibição de um conjunto de registros do banco de dados. Mostrar dois métodos de formatação de um conjunto de registros do banco de dados em um HTML ta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: dd871520f98aaae2d7b33d83b1646eb9eee88821
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872267"
---
<a name="displaying-a-table-of-database-data-vb"></a><span data-ttu-id="038cf-104">Exibindo uma tabela de banco de dados (VB)</span><span class="sxs-lookup"><span data-stu-id="038cf-104">Displaying a Table of Database Data (VB)</span></span>
====================
<span data-ttu-id="038cf-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="038cf-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="038cf-106">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="038cf-106">Download PDF</span></span>](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> <span data-ttu-id="038cf-107">Neste tutorial, demonstrarei dois métodos de exibição de um conjunto de registros do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="038cf-107">In this tutorial, I demonstrate two methods of displaying a set of database records.</span></span> <span data-ttu-id="038cf-108">Vou mostrar dois métodos de formatação de um conjunto de registros do banco de dados em uma tabela HTML.</span><span class="sxs-lookup"><span data-stu-id="038cf-108">I show two methods of formatting a set of database records in an HTML table.</span></span> <span data-ttu-id="038cf-109">Primeiro, vou mostrar como você pode formatar os registros de banco de dados diretamente dentro de um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="038cf-109">First, I show how you can format the database records directly within a view.</span></span> <span data-ttu-id="038cf-110">Em seguida, demonstrarei como você pode tirar proveito das existe meio-termo ao formatar os registros do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="038cf-110">Next, I demonstrate how you can take advantage of partials when formatting database records.</span></span>


<span data-ttu-id="038cf-111">O objetivo deste tutorial é explicar como você pode exibir uma tabela HTML do banco de dados em um aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="038cf-111">The goal of this tutorial is to explain how you can display an HTML table of database data in an ASP.NET MVC application.</span></span> <span data-ttu-id="038cf-112">Primeiro, você aprenderá a usar as ferramentas de scaffolding incluídas no Visual Studio para gerar uma exibição que exibe um conjunto de registros automaticamente.</span><span class="sxs-lookup"><span data-stu-id="038cf-112">First, you learn how to use the scaffolding tools included in Visual Studio to generate a view that displays a set of records automatically.</span></span> <span data-ttu-id="038cf-113">Em seguida, você aprenderá a usar um parcial como um modelo quando os registros do banco de dados de formatação.</span><span class="sxs-lookup"><span data-stu-id="038cf-113">Next, you learn how to use a partial as a template when formatting database records.</span></span>

## <a name="create-the-model-classes"></a><span data-ttu-id="038cf-114">Criar as Classes de modelo</span><span class="sxs-lookup"><span data-stu-id="038cf-114">Create the Model Classes</span></span>

<span data-ttu-id="038cf-115">Vamos para exibir o conjunto de registros da tabela de banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="038cf-115">We are going to display the set of records from the Movies database table.</span></span> <span data-ttu-id="038cf-116">A tabela de banco de dados de filmes contém as seguintes colunas:</span><span class="sxs-lookup"><span data-stu-id="038cf-116">The Movies database table contains the following columns:</span></span>

<a id="0.4_table01"></a>


| <span data-ttu-id="038cf-117">**Nome da coluna**</span><span class="sxs-lookup"><span data-stu-id="038cf-117">**Column Name**</span></span> | <span data-ttu-id="038cf-118">**Tipo de dados**</span><span class="sxs-lookup"><span data-stu-id="038cf-118">**Data Type**</span></span> | <span data-ttu-id="038cf-119">**Permitir nulos**</span><span class="sxs-lookup"><span data-stu-id="038cf-119">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="038cf-120">Id</span><span class="sxs-lookup"><span data-stu-id="038cf-120">Id</span></span> | <span data-ttu-id="038cf-121">int</span><span class="sxs-lookup"><span data-stu-id="038cf-121">Int</span></span> | <span data-ttu-id="038cf-122">False</span><span class="sxs-lookup"><span data-stu-id="038cf-122">False</span></span> |
| <span data-ttu-id="038cf-123">Título</span><span class="sxs-lookup"><span data-stu-id="038cf-123">Title</span></span> | <span data-ttu-id="038cf-124">Nvarchar(200)</span><span class="sxs-lookup"><span data-stu-id="038cf-124">Nvarchar(200)</span></span> | <span data-ttu-id="038cf-125">False</span><span class="sxs-lookup"><span data-stu-id="038cf-125">False</span></span> |
| <span data-ttu-id="038cf-126">Diretor</span><span class="sxs-lookup"><span data-stu-id="038cf-126">Director</span></span> | <span data-ttu-id="038cf-127">NVarchar(50)</span><span class="sxs-lookup"><span data-stu-id="038cf-127">NVarchar(50)</span></span> | <span data-ttu-id="038cf-128">False</span><span class="sxs-lookup"><span data-stu-id="038cf-128">False</span></span> |
| <span data-ttu-id="038cf-129">DateReleased</span><span class="sxs-lookup"><span data-stu-id="038cf-129">DateReleased</span></span> | <span data-ttu-id="038cf-130">DateTime</span><span class="sxs-lookup"><span data-stu-id="038cf-130">DateTime</span></span> | <span data-ttu-id="038cf-131">False</span><span class="sxs-lookup"><span data-stu-id="038cf-131">False</span></span> |


<span data-ttu-id="038cf-132">Para representar a tabela de filmes em nosso aplicativo ASP.NET MVC, precisamos criar uma classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="038cf-132">In order to represent the Movies table in our ASP.NET MVC application, we need to create a model class.</span></span> <span data-ttu-id="038cf-133">Neste tutorial, usamos o Entity Framework da Microsoft para criar nossas classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="038cf-133">In this tutorial, we use the Microsoft Entity Framework to create our model classes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="038cf-134">Neste tutorial, usamos o Entity Framework da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="038cf-134">In this tutorial, we use the Microsoft Entity Framework.</span></span> <span data-ttu-id="038cf-135">No entanto, é importante entender que você pode usar uma variedade de tecnologias diferentes para interagir com um banco de dados de um aplicativo ASP.NET MVC incluindo LINQ to SQL, o NHibernate ou ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="038cf-135">However, it is important to understand that you can use a variety of different technologies to interact with a database from an ASP.NET MVC application including LINQ to SQL, NHibernate, or ADO.NET.</span></span>


<span data-ttu-id="038cf-136">Siga estas etapas para iniciar o Assistente de modelo de dados de entidade:</span><span class="sxs-lookup"><span data-stu-id="038cf-136">Follow these steps to launch the Entity Data Model Wizard:</span></span>

1. <span data-ttu-id="038cf-137">Clique na pasta de modelos na janela do Gerenciador de soluções e selecione a opção de menu **adicionar, o novo Item**.</span><span class="sxs-lookup"><span data-stu-id="038cf-137">Right-click the Models folder in the Solution Explorer window and the select the menu option **Add, New Item**.</span></span>
2. <span data-ttu-id="038cf-138">Selecione o **dados** categoria e selecione o **modelo de dados de entidade ADO.NET** modelo.</span><span class="sxs-lookup"><span data-stu-id="038cf-138">Select the **Data** category and select the **ADO.NET Entity Data Model** template.</span></span>
3. <span data-ttu-id="038cf-139">Forneça o nome de seu modelo de dados *MoviesDBModel.edmx* e clique no **adicionar** botão.</span><span class="sxs-lookup"><span data-stu-id="038cf-139">Give your data model the name *MoviesDBModel.edmx* and click the **Add** button.</span></span>

<span data-ttu-id="038cf-140">Depois de clicar no botão Adicionar, o Assistente de modelo de dados de entidade é exibida (consulte a Figura 1).</span><span class="sxs-lookup"><span data-stu-id="038cf-140">After you click the Add button, the Entity Data Model Wizard appears (see Figure 1).</span></span> <span data-ttu-id="038cf-141">Siga estas etapas para concluir o Assistente:</span><span class="sxs-lookup"><span data-stu-id="038cf-141">Follow these steps to complete the wizard:</span></span>

1. <span data-ttu-id="038cf-142">No **escolher o modelo de conteúdo** etapa, selecione o **gerar do banco de dados** opção.</span><span class="sxs-lookup"><span data-stu-id="038cf-142">In the **Choose Model Contents** step, select the **Generate from database** option.</span></span>
2. <span data-ttu-id="038cf-143">No **escolha sua Conexão de dados** etapa, use o *MoviesDB.mdf* conexão de dados e o nome *MoviesDBEntities* para as configurações de conexão.</span><span class="sxs-lookup"><span data-stu-id="038cf-143">In the **Choose Your Data Connection** step, use the *MoviesDB.mdf* data connection and the name *MoviesDBEntities* for the connection settings.</span></span> <span data-ttu-id="038cf-144">Clique o **próximo** botão.</span><span class="sxs-lookup"><span data-stu-id="038cf-144">Click the **Next** button.</span></span>
3. <span data-ttu-id="038cf-145">No **escolher seus objetos de banco de dados** etapa, expanda o nó tabelas, selecione a tabela de filmes.</span><span class="sxs-lookup"><span data-stu-id="038cf-145">In the **Choose Your Database Objects** step, expand the Tables node, select the Movies table.</span></span> <span data-ttu-id="038cf-146">Insira o namespace *modelos* e clique no **concluir** botão.</span><span class="sxs-lookup"><span data-stu-id="038cf-146">Enter the namespace *Models* and click the **Finish** button.</span></span>


<span data-ttu-id="038cf-147">[![Criando LINQ para classes do SQL](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="038cf-147">[![Creating LINQ to SQL classes](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)</span></span>

<span data-ttu-id="038cf-148">**Figura 01**: Criando classes LINQ to SQL ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="038cf-148">**Figure 01**: Creating LINQ to SQL classes ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image2.png))</span></span>


<span data-ttu-id="038cf-149">Depois de concluir o Assistente de modelo de dados de entidade, abre o Designer de modelo de dados de entidade.</span><span class="sxs-lookup"><span data-stu-id="038cf-149">After you complete the Entity Data Model Wizard, the Entity Data Model Designer opens.</span></span> <span data-ttu-id="038cf-150">O Designer deve exibir a entidade de filmes (consulte a Figura 2).</span><span class="sxs-lookup"><span data-stu-id="038cf-150">The Designer should display the Movies entity (see Figure 2).</span></span>


<span data-ttu-id="038cf-151">[![O Designer de modelo de dados de entidade](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="038cf-151">[![The Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)</span></span>

<span data-ttu-id="038cf-152">**Figura 02**: O Designer de modelo de dados de entidade ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="038cf-152">**Figure 02**: The Entity Data Model Designer ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image4.png))</span></span>


<span data-ttu-id="038cf-153">É preciso fazer uma alteração antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="038cf-153">We need to make one change before we continue.</span></span> <span data-ttu-id="038cf-154">O Assistente de dados de entidade gera uma classe de modelo chamada *filmes* que representa a tabela de banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="038cf-154">The Entity Data Wizard generates a model class named *Movies* that represents the Movies database table.</span></span> <span data-ttu-id="038cf-155">Como vamos usar a classe de filmes para representar um filme específico, é preciso modificar o nome da classe a ser *filme* em vez de *filmes* (singular em vez de no plural).</span><span class="sxs-lookup"><span data-stu-id="038cf-155">Because we'll use the Movies class to represent a particular movie, we need to modify the name of the class to be *Movie* instead of *Movies* (singular rather than plural).</span></span>

<span data-ttu-id="038cf-156">Clique duas vezes no nome da classe na superfície do designer e alterar o nome da classe de filmes para filme.</span><span class="sxs-lookup"><span data-stu-id="038cf-156">Double-click the name of the class on the designer surface and change the name of the class from Movies to Movie.</span></span> <span data-ttu-id="038cf-157">Depois de fazer essa alteração, clique no **salvar** botão (o ícone de disquete) para gerar a classe de filme.</span><span class="sxs-lookup"><span data-stu-id="038cf-157">After making this change, click the **Save** button (the icon of the floppy disk) to generate the Movie class.</span></span>

## <a name="create-the-movies-controller"></a><span data-ttu-id="038cf-158">Criar o controlador de filmes</span><span class="sxs-lookup"><span data-stu-id="038cf-158">Create the Movies Controller</span></span>

<span data-ttu-id="038cf-159">Agora que temos uma maneira de representar nossos registros do banco de dados, podemos criar um controlador que retorna a coleção de filmes.</span><span class="sxs-lookup"><span data-stu-id="038cf-159">Now that we have a way to represent our database records, we can create a controller that returns the collection of movies.</span></span> <span data-ttu-id="038cf-160">Dentro da janela do Gerenciador de soluções do Visual Studio, clique na pasta controladores e selecione a opção de menu **adicionar, controlador** (consulte a Figura 3).</span><span class="sxs-lookup"><span data-stu-id="038cf-160">Within the Visual Studio Solution Explorer window, right-click the Controllers folder and select the menu option **Add, Controller** (see Figure 3).</span></span>


<span data-ttu-id="038cf-161">[![O Menu do controlador de adição](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="038cf-161">[![The Add Controller Menu](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)</span></span>

<span data-ttu-id="038cf-162">**Figura 03**: Adicionar controlador Menu ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="038cf-162">**Figure 03**: The Add Controller Menu ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image6.png))</span></span>


<span data-ttu-id="038cf-163">Quando o **Adicionar controlador** caixa de diálogo for exibida, insira o nome do controlador MovieController (consulte a Figura 4).</span><span class="sxs-lookup"><span data-stu-id="038cf-163">When the **Add Controller** dialog appears, enter the controller name MovieController (see Figure 4).</span></span> <span data-ttu-id="038cf-164">Clique o **adicionar** botão para adicionar o novo controlador.</span><span class="sxs-lookup"><span data-stu-id="038cf-164">Click the **Add** button to add the new controller.</span></span>


<span data-ttu-id="038cf-165">[![A caixa de diálogo Adicionar controlador](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="038cf-165">[![The Add Controller dialog](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)</span></span>

<span data-ttu-id="038cf-166">**Figura 04**: caixa de diálogo a adicionar controlador ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="038cf-166">**Figure 04**: The Add Controller dialog([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image8.png))</span></span>


<span data-ttu-id="038cf-167">É preciso modificar a ação de Index () exposta pelo controlador de filme, de forma que ele retorna o conjunto de registros do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="038cf-167">We need to modify the Index() action exposed by the Movie controller so that it returns the set of database records.</span></span> <span data-ttu-id="038cf-168">Modificar o controlador para que ele se parece com o controlador na listagem 1.</span><span class="sxs-lookup"><span data-stu-id="038cf-168">Modify the controller so that it looks like the controller in Listing 1.</span></span>

<span data-ttu-id="038cf-169">**Listando 1 – Controllers\MovieController.vb**</span><span class="sxs-lookup"><span data-stu-id="038cf-169">**Listing 1 – Controllers\MovieController.vb**</span></span>

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

<span data-ttu-id="038cf-170">Na listagem 1, a classe MoviesDBEntities é usada para representar o banco de dados MoviesDB.</span><span class="sxs-lookup"><span data-stu-id="038cf-170">In Listing 1, the MoviesDBEntities class is used to represent the MoviesDB database.</span></span> <span data-ttu-id="038cf-171">A expressão *entidades. MovieSet.ToList()* retorna o conjunto de todos os filmes da tabela de banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="038cf-171">The expression *entities.MovieSet.ToList()* returns the set of all movies from the Movies database table.</span></span>

## <a name="create-the-view"></a><span data-ttu-id="038cf-172">Criar o modo de exibição</span><span class="sxs-lookup"><span data-stu-id="038cf-172">Create the View</span></span>

<span data-ttu-id="038cf-173">É a maneira mais fácil de exibir um conjunto de registros do banco de dados em uma tabela HTML aproveitar o scaffolding fornecido pelo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="038cf-173">The easiest way to display a set of database records in an HTML table is to take advantage of the scaffolding provided by Visual Studio.</span></span>

<span data-ttu-id="038cf-174">Criar seu aplicativo, selecionando a opção de menu **criar, compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="038cf-174">Build your application by selecting the menu option **Build, Build Solution**.</span></span> <span data-ttu-id="038cf-175">Você deve criar seu aplicativo antes de abrir o **adicionar exibição** suas classes de dados ou de caixa de diálogo não aparecerão na caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="038cf-175">You must build your application before opening the **Add View** dialog or your data classes won't appear in the dialog.</span></span>

<span data-ttu-id="038cf-176">A ação Index () e selecione a opção de menu **adicionar exibição** (consulte a Figura 5).</span><span class="sxs-lookup"><span data-stu-id="038cf-176">Right-click the Index() action and select the menu option **Add View** (see Figure 5).</span></span>


<span data-ttu-id="038cf-177">[![Adicionando uma exibição](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="038cf-177">[![Adding a view](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)</span></span>

<span data-ttu-id="038cf-178">**Figura 05**: adicionando uma exibição ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="038cf-178">**Figure 05**: Adding a view ([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image10.png))</span></span>


<span data-ttu-id="038cf-179">No **adicionar exibição** caixa de diálogo, marque a caixa de seleção **criar uma exibição fortemente tipada**.</span><span class="sxs-lookup"><span data-stu-id="038cf-179">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="038cf-180">Selecione a classe de filme como o **exibir dados de classe**.</span><span class="sxs-lookup"><span data-stu-id="038cf-180">Select the Movie class as the **view data class**.</span></span> <span data-ttu-id="038cf-181">Selecione *lista* como o **exibir conteúdo** (veja a Figura 6).</span><span class="sxs-lookup"><span data-stu-id="038cf-181">Select *List* as the **view content** (see Figure 6).</span></span> <span data-ttu-id="038cf-182">Selecionar essas opções irá gerar uma exibição fortemente tipada que exibe uma lista de filmes.</span><span class="sxs-lookup"><span data-stu-id="038cf-182">Selecting these options will generate a strongly-typed view that displays a list of movies.</span></span>


<span data-ttu-id="038cf-183">[![A caixa de diálogo Adicionar modo de exibição](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="038cf-183">[![The Add View dialog](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)</span></span>

<span data-ttu-id="038cf-184">**Figura 06**: caixa de diálogo Adicionar exibir ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="038cf-184">**Figure 06**: The Add View dialog([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image12.png))</span></span>


<span data-ttu-id="038cf-185">Depois de clicar no **adicionar** botão, o modo de exibição na lista 2 é gerado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="038cf-185">After you click the **Add** button, the view in Listing 2 is generated automatically.</span></span> <span data-ttu-id="038cf-186">Essa exibição contém o código necessário para iterar pela coleção de filmes e exibir cada uma das propriedades de um filme.</span><span class="sxs-lookup"><span data-stu-id="038cf-186">This view contains the code required to iterate through the collection of movies and display each of the properties of a movie.</span></span>

<span data-ttu-id="038cf-187">**Listing 2 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="038cf-187">**Listing 2 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

<span data-ttu-id="038cf-188">Você pode executar o aplicativo, selecionando a opção de menu **depurar, iniciar depuração** (ou pressionar a tecla F5).</span><span class="sxs-lookup"><span data-stu-id="038cf-188">You can run the application by selecting the menu option **Debug, Start Debugging** (or hitting the F5 key).</span></span> <span data-ttu-id="038cf-189">Executando o aplicativo inicia o Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="038cf-189">Running the application launches Internet Explorer.</span></span> <span data-ttu-id="038cf-190">Se você navegar para a URL de /Movie, em seguida, você verá a página na Figura 7.</span><span class="sxs-lookup"><span data-stu-id="038cf-190">If you navigate to the /Movie URL then you'll see the page in Figure 7.</span></span>


<span data-ttu-id="038cf-191">[![Uma tabela de filmes](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="038cf-191">[![A table of movies](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)</span></span>

<span data-ttu-id="038cf-192">**Figura 07**: uma tabela de filmes ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="038cf-192">**Figure 07**: A table of movies([Click to view full-size image](displaying-a-table-of-database-data-vb/_static/image14.png))</span></span>


<span data-ttu-id="038cf-193">Se você não gosta nada sobre a aparência da grade de registros do banco de dados na Figura 7, em seguida, você simplesmente pode modificar a exibição do índice.</span><span class="sxs-lookup"><span data-stu-id="038cf-193">If you don't like anything about the appearance of the grid of database records in Figure 7 then you can simply modify the Index view.</span></span> <span data-ttu-id="038cf-194">Por exemplo, você pode alterar o *DateReleased* cabeçalho para *data de lançamento* modificando a exibição do índice.</span><span class="sxs-lookup"><span data-stu-id="038cf-194">For example, you can change the *DateReleased* header to *Date Released* by modifying the Index view.</span></span>

## <a name="create-a-template-with-a-partial"></a><span data-ttu-id="038cf-195">Criar um modelo com um parcial</span><span class="sxs-lookup"><span data-stu-id="038cf-195">Create a Template with a Partial</span></span>

<span data-ttu-id="038cf-196">Quando uma exibição é muito complicada, é recomendável iniciar invadir o modo de exibição existe meio-termo.</span><span class="sxs-lookup"><span data-stu-id="038cf-196">When a view gets too complicated, it is a good idea to start breaking the view into partials.</span></span> <span data-ttu-id="038cf-197">Usar existe meio-termo torna mais fácil de entender e manter seus modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="038cf-197">Using partials makes your views easier to understand and maintain.</span></span> <span data-ttu-id="038cf-198">Vamos criar um parcial que podemos usar como modelo para formatar cada um dos registros de banco de dados do filme.</span><span class="sxs-lookup"><span data-stu-id="038cf-198">We'll create a partial that we can use as a template to format each of the movie database records.</span></span>

<span data-ttu-id="038cf-199">Siga estas etapas para criar o parcial:</span><span class="sxs-lookup"><span data-stu-id="038cf-199">Follow these steps to create the partial:</span></span>

1. <span data-ttu-id="038cf-200">Clique na pasta Views\Movie e selecione a opção de menu **adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="038cf-200">Right-click the Views\Movie folder and select the menu option **Add View**.</span></span>
2. <span data-ttu-id="038cf-201">Marque a caixa de seleção *criar uma exibição parcial (. ascx)*.</span><span class="sxs-lookup"><span data-stu-id="038cf-201">Check the checkbox labeled *Create a partial view (.ascx)*.</span></span>
3. <span data-ttu-id="038cf-202">Nomeie o parcial *MovieTemplate*.</span><span class="sxs-lookup"><span data-stu-id="038cf-202">Name the partial *MovieTemplate*.</span></span>
4. <span data-ttu-id="038cf-203">Marque a caixa de seleção **criar uma exibição fortemente tipada**.</span><span class="sxs-lookup"><span data-stu-id="038cf-203">Check the checkbox labeled **Create a strongly-typed view**.</span></span>
5. <span data-ttu-id="038cf-204">Selecione um filme, como o *exibir dados de classe*.</span><span class="sxs-lookup"><span data-stu-id="038cf-204">Select Movie as the *view data class*.</span></span>
6. <span data-ttu-id="038cf-205">Selecione vazio como o *exibir conteúdo*.</span><span class="sxs-lookup"><span data-stu-id="038cf-205">Select Empty as the *view content*.</span></span>
7. <span data-ttu-id="038cf-206">Clique o **adicionar** botão para adicionar o parcial para seu projeto.</span><span class="sxs-lookup"><span data-stu-id="038cf-206">Click the **Add** button to add the partial to your project.</span></span>

<span data-ttu-id="038cf-207">Depois de concluir essas etapas, modifique o MovieTemplate parcial para se parecer com a listagem 3.</span><span class="sxs-lookup"><span data-stu-id="038cf-207">After you complete these steps, modify the MovieTemplate partial to look like Listing 3.</span></span>

<span data-ttu-id="038cf-208">**A listagem 3 – Views\Movie\MovieTemplate.ascx**</span><span class="sxs-lookup"><span data-stu-id="038cf-208">**Listing 3 – Views\Movie\MovieTemplate.ascx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

<span data-ttu-id="038cf-209">Parcial na listagem 3 contém um modelo para uma única linha de registros.</span><span class="sxs-lookup"><span data-stu-id="038cf-209">The partial in Listing 3 contains a template for a single row of records.</span></span>

<span data-ttu-id="038cf-210">O modo de exibição do índice modificado na listagem 4 usa o MovieTemplate parcial.</span><span class="sxs-lookup"><span data-stu-id="038cf-210">The modified Index view in Listing 4 uses the MovieTemplate partial.</span></span>

<span data-ttu-id="038cf-211">**A listagem 4 – Views\Movie\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="038cf-211">**Listing 4 – Views\Movie\Index.aspx**</span></span>

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

<span data-ttu-id="038cf-212">O modo de exibição na listagem 4 contém um para cada loop que itera por meio de todos os filmes.</span><span class="sxs-lookup"><span data-stu-id="038cf-212">The view in Listing 4 contains a For Each loop that iterates through all of the movies.</span></span> <span data-ttu-id="038cf-213">Para cada filme, o MovieTemplate parcial é usado para formatar o filme.</span><span class="sxs-lookup"><span data-stu-id="038cf-213">For each movie, the MovieTemplate partial is used to format the movie.</span></span> <span data-ttu-id="038cf-214">O MovieTemplate é renderizado, chamando o método auxiliar RenderPartial().</span><span class="sxs-lookup"><span data-stu-id="038cf-214">The MovieTemplate is rendered by calling the RenderPartial() helper method.</span></span>

<span data-ttu-id="038cf-215">O modo de exibição do índice modificado renderiza a mesma tabela HTML de registros do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="038cf-215">The modified Index view renders the very same HTML table of database records.</span></span> <span data-ttu-id="038cf-216">No entanto, o modo de exibição foi bastante simplificado.</span><span class="sxs-lookup"><span data-stu-id="038cf-216">However, the view has been greatly simplified.</span></span>


<span data-ttu-id="038cf-217">O método RenderPartial() é diferente do que a maioria dos outros métodos auxiliares porque ele não retorna uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="038cf-217">The RenderPartial() method is different than most of the other helper methods because it does not return a string.</span></span> <span data-ttu-id="038cf-218">Portanto, você deve chamar o método RenderPartial() usando &lt;Html.RenderPartial() %&gt; em vez de &lt;% = % Html.RenderPartial()&gt;.</span><span class="sxs-lookup"><span data-stu-id="038cf-218">Therefore, you must call the RenderPartial() method using &lt;% Html.RenderPartial() %&gt; instead of &lt;%= Html.RenderPartial() %&gt;.</span></span>


## <a name="summary"></a><span data-ttu-id="038cf-219">Resumo</span><span class="sxs-lookup"><span data-stu-id="038cf-219">Summary</span></span>

<span data-ttu-id="038cf-220">O objetivo deste tutorial era explicam como você pode exibir um conjunto de registros do banco de dados em uma tabela HTML.</span><span class="sxs-lookup"><span data-stu-id="038cf-220">The goal of this tutorial was to explain how you can display a set of database records in an HTML table.</span></span> <span data-ttu-id="038cf-221">Primeiro, você aprendeu como retornar um conjunto de registros do banco de dados de uma ação do controlador, aproveitando o Entity Framework da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="038cf-221">First, you learned how to return a set of database records from a controller action by taking advantage of the Microsoft Entity Framework.</span></span> <span data-ttu-id="038cf-222">Em seguida, você aprendeu a usar scaffolding do Visual Studio para gerar uma exibição que exibe uma coleção de itens automaticamente.</span><span class="sxs-lookup"><span data-stu-id="038cf-222">Next, you learned how to use Visual Studio scaffolding to generate a view that displays a collection of items automatically.</span></span> <span data-ttu-id="038cf-223">Por fim, você aprendeu a simplificar a exibição aproveitando um parcial.</span><span class="sxs-lookup"><span data-stu-id="038cf-223">Finally, you learned how to simplify the view by taking advantage of a partial.</span></span> <span data-ttu-id="038cf-224">Você aprendeu a usar um parcial como um modelo para que você pode formatar cada registro do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="038cf-224">You learned how to use a partial as a template so that you can format each database record.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="038cf-225">[Anterior](creating-model-classes-with-linq-to-sql-vb.md)
> [Próximo](performing-simple-validation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="038cf-225">[Previous](creating-model-classes-with-linq-to-sql-vb.md)
[Next](performing-simple-validation-vb.md)</span></span>

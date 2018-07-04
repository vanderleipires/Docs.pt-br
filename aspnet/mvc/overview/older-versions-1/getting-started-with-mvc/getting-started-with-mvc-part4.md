---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Criando um banco de dados | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que lê e grava de um banco de dados.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 0e72359095e4c40ef7e56f1290a45ded257143c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362350"
---
<a name="creating-a-database"></a><span data-ttu-id="9227e-104">Criando um banco de dados</span><span class="sxs-lookup"><span data-stu-id="9227e-104">Creating a Database</span></span>
====================
<span data-ttu-id="9227e-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="9227e-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="9227e-106">Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9227e-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="9227e-107">Você criará um aplicativo web simples que lê e grava de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9227e-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="9227e-108">Visite o [Central de informações do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.</span><span class="sxs-lookup"><span data-stu-id="9227e-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="9227e-109">Nesta seção, vamos criar um novo SQL Express banco de dados que usaremos para armazenar e recuperar os dados de filme.</span><span class="sxs-lookup"><span data-stu-id="9227e-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="9227e-110">No IDE do Visual Web Developer, selecione Exibir | Gerenciador de servidores.</span><span class="sxs-lookup"><span data-stu-id="9227e-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="9227e-111">Clique com o botão direito em conexões de dados e clique em Adicionar Conexão...</span><span class="sxs-lookup"><span data-stu-id="9227e-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="9227e-113">Na caixa de diálogo Escolher fonte de dados, selecione Microsoft SQL Server e selecione continuar.</span><span class="sxs-lookup"><span data-stu-id="9227e-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="9227e-114">Na caixa de diálogo Adicionar Conexão, insira ". \SQLEXPRESS" para o nome do servidor e digite "Filmes" como o nome do novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9227e-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="9227e-115">[![Adicionar caixa de diálogo de Conexão](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="9227e-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="9227e-116">Clique em Okey e você será solicitado se você deseja criar esse banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9227e-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="9227e-117">Selecione Sim.</span><span class="sxs-lookup"><span data-stu-id="9227e-117">Select yes.</span></span>

<span data-ttu-id="9227e-118">[![Criar filmes?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="9227e-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="9227e-119">Agora você tem um banco de dados vazio no Gerenciador de servidores.</span><span class="sxs-lookup"><span data-stu-id="9227e-119">Now you've got an empty database in Server Explorer.</span></span>

![Adicionar nova tabela](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="9227e-121">Clique com botão direito em tabelas e clique em Adicionar tabela.</span><span class="sxs-lookup"><span data-stu-id="9227e-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="9227e-122">O Designer de tabela será exibida.</span><span class="sxs-lookup"><span data-stu-id="9227e-122">The Table Designer will appear.</span></span> <span data-ttu-id="9227e-123">Adicione colunas para Id, título, ReleaseDate, gênero e preço.</span><span class="sxs-lookup"><span data-stu-id="9227e-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="9227e-124">Clique com o botão direito na coluna de ID e clique em define chave primária.</span><span class="sxs-lookup"><span data-stu-id="9227e-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="9227e-125">É aqui que minhas áreas de design é semelhante.</span><span class="sxs-lookup"><span data-stu-id="9227e-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="9227e-126">[![Editor de tabela de banco de dados](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="9227e-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="9227e-127">Além disso, selecione a coluna de Id e em Propriedades de coluna abaixo, altere "Especificação de identidade" para "Sim".</span><span class="sxs-lookup"><span data-stu-id="9227e-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="9227e-128">[![IsIdentity - propriedades da coluna](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="9227e-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="9227e-129">Quando você tem feito, clique no ícone Salvar na barra de ferramentas ou selecione arquivo | Salvar no menu e nomear sua tabela "**filme**" (singular).</span><span class="sxs-lookup"><span data-stu-id="9227e-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="9227e-130">Temos um banco de dados e uma tabela!</span><span class="sxs-lookup"><span data-stu-id="9227e-130">We've got a database and a table!</span></span>

<span data-ttu-id="9227e-131">[![Escolher nome](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="9227e-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="9227e-132">Volte para o Gerenciador de servidores, clique com botão direito tabela Movie e selecione "Mostrar dados da tabela".</span><span class="sxs-lookup"><span data-stu-id="9227e-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="9227e-133">Insira alguns filmes para que nosso banco de dados tenha alguns dados.</span><span class="sxs-lookup"><span data-stu-id="9227e-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="9227e-134">[![Edição de tabela do banco de dados](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="9227e-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="9227e-135">Criar um Modelo</span><span class="sxs-lookup"><span data-stu-id="9227e-135">Creating a Model</span></span>

<span data-ttu-id="9227e-136">Agora, volte para o Gerenciador de soluções no lado direito do IDE e com o botão direito na pasta modelos e selecione Adicionar | Novo Item.</span><span class="sxs-lookup"><span data-stu-id="9227e-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="9227e-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="9227e-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="9227e-138">Vamos criar um modelo de entidade de nosso novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9227e-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="9227e-139">Isso adicionará um conjunto de classes ao nosso projeto que torna mais fácil para nós consultar e manipular os dados dentro de nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9227e-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="9227e-140">Selecione o nó de dados no lado esquerdo da caixa de diálogo e, em seguida, selecione o modelo de item de modelo de dados de entidade ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="9227e-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="9227e-141">O nome Movies.edmx.</span><span class="sxs-lookup"><span data-stu-id="9227e-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="9227e-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="9227e-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="9227e-143">Clique no botão "Adicionar".</span><span class="sxs-lookup"><span data-stu-id="9227e-143">Click the "Add" button.</span></span> <span data-ttu-id="9227e-144">Isso iniciará, em seguida, "Entidade de dados modelo de assistente".</span><span class="sxs-lookup"><span data-stu-id="9227e-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="9227e-145">Na nova caixa de diálogo pop-up, selecione Gerar do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9227e-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="9227e-146">Desde que acabamos de criar um banco de dados, só precisaremos dizer o Entity Framework sobre nosso novo banco de dados e sua tabela.</span><span class="sxs-lookup"><span data-stu-id="9227e-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="9227e-147">Clique em próximo a salvar nossa conexão de banco de dados na configuração do nosso aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="9227e-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="9227e-148">Agora, marque as tabelas e o filme caixa de seleção e clique em Finish.</span><span class="sxs-lookup"><span data-stu-id="9227e-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="9227e-149">[![Assistente de modelo de dados de entidade](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="9227e-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="9227e-150">Agora podemos ver nossa nova tabela de filme no Entity Framework Designer e acessá-lo a partir do código.</span><span class="sxs-lookup"><span data-stu-id="9227e-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="9227e-151">[![Filmes - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="9227e-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="9227e-152">Na superfície de design, você pode ver uma classe "Filme".</span><span class="sxs-lookup"><span data-stu-id="9227e-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="9227e-153">Essa classe é mapeada para a tabela "Filme" em nosso banco de dados, e cada propriedade dentro dele é mapeado para uma coluna com a tabela.</span><span class="sxs-lookup"><span data-stu-id="9227e-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="9227e-154">Cada instância de uma classe "Filme" corresponderá a uma linha dentro da tabela "Filme".</span><span class="sxs-lookup"><span data-stu-id="9227e-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="9227e-155">Se você não gostar de padrão de nomenclatura e convenções usadas pelo Entity Framework de mapeamento, você pode usar o designer do Entity Framework para alterar ou personalizá-los.</span><span class="sxs-lookup"><span data-stu-id="9227e-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="9227e-156">Para este aplicativo usaremos os padrões e salvar o arquivo como-está.</span><span class="sxs-lookup"><span data-stu-id="9227e-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="9227e-157">Agora, vamos trabalhar com alguns dados reais!</span><span class="sxs-lookup"><span data-stu-id="9227e-157">Now, let's work with some real data!</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9227e-158">[Anterior](getting-started-with-mvc-part3.md)
> [Próximo](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="9227e-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>

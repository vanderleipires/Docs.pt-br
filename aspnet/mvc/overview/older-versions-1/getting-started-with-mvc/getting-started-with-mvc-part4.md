---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Criando um banco de dados | Microsoft Docs
author: shanselman
description: "Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Você criará um aplicativo web simples que leituras e gravações de banco de dados."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 6a9617b8056f883d6be2547588b13063a58abd0e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-database"></a><span data-ttu-id="e2267-104">Criando um banco de dados</span><span class="sxs-lookup"><span data-stu-id="e2267-104">Creating a Database</span></span>
====================
<span data-ttu-id="e2267-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="e2267-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="e2267-106">Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e2267-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="e2267-107">Você criará um aplicativo web simples que leituras e gravações de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e2267-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="e2267-108">Visite o [Central de aprendizagem do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.</span><span class="sxs-lookup"><span data-stu-id="e2267-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="e2267-109">Nesta seção, vamos criar um novo SQL Express banco de dados que usaremos para armazenar e recuperar os dados do filme.</span><span class="sxs-lookup"><span data-stu-id="e2267-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="e2267-110">Modo de exibição no IDE do Visual Web Developer selecione | Gerenciador de servidores.</span><span class="sxs-lookup"><span data-stu-id="e2267-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="e2267-111">Clique com o botão direito em conexões de dados e clique em Adicionar Conexão...</span><span class="sxs-lookup"><span data-stu-id="e2267-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="e2267-113">Na caixa de diálogo Escolher fonte de dados, selecione Microsoft SQL Server e selecione continuar.</span><span class="sxs-lookup"><span data-stu-id="e2267-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="e2267-114">Na caixa de diálogo Adicionar Conexão, digite ". \SQLEXPRESS" para o nome do servidor e digite "Filmes" como o nome do novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e2267-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="e2267-115">[![Adicionar caixa de diálogo de Conexão](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="e2267-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="e2267-116">Clique em Okey e será perguntado se você deseja criar esse banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e2267-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="e2267-117">Selecione Sim.</span><span class="sxs-lookup"><span data-stu-id="e2267-117">Select yes.</span></span>

<span data-ttu-id="e2267-118">[![Criar filmes?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="e2267-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="e2267-119">Agora você tem um banco de dados vazio no Gerenciador de servidores.</span><span class="sxs-lookup"><span data-stu-id="e2267-119">Now you've got an empty database in Server Explorer.</span></span>

![Adicionar nova tabela](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="e2267-121">Clique com o botão direito em tabelas e clique em Adicionar tabela.</span><span class="sxs-lookup"><span data-stu-id="e2267-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="e2267-122">O Designer de tabela será exibida.</span><span class="sxs-lookup"><span data-stu-id="e2267-122">The Table Designer will appear.</span></span> <span data-ttu-id="e2267-123">Adicione colunas de Id, título, ReleaseDate, gênero e preço.</span><span class="sxs-lookup"><span data-stu-id="e2267-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="e2267-124">Clique com o botão direito na coluna de ID e clique em define chave primária.</span><span class="sxs-lookup"><span data-stu-id="e2267-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="e2267-125">É aqui que meu áreas de design parece.</span><span class="sxs-lookup"><span data-stu-id="e2267-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="e2267-126">[![Editor de tabela de banco de dados](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="e2267-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="e2267-127">Além disso, selecione a coluna de Id e em Propriedades da coluna abaixo, altere "Especificação de identidade" para "Sim".</span><span class="sxs-lookup"><span data-stu-id="e2267-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="e2267-128">[![IsIdentity - propriedades da coluna](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="e2267-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="e2267-129">Quando você tem feito, clique no ícone Salvar na barra de ferramentas ou selecione arquivo | Salvar no menu e nomeie a tabela "**filme**" (singular).</span><span class="sxs-lookup"><span data-stu-id="e2267-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="e2267-130">Temos um banco de dados e uma tabela!</span><span class="sxs-lookup"><span data-stu-id="e2267-130">We've got a database and a table!</span></span>

<span data-ttu-id="e2267-131">[![Escolher nome](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="e2267-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="e2267-132">Vá para Gerenciador de servidores, a tabela de filme clique com botão direito e selecione "Mostrar dados da tabela".</span><span class="sxs-lookup"><span data-stu-id="e2267-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="e2267-133">Insira algumas filmes para que nosso banco de dados tenha alguns dados.</span><span class="sxs-lookup"><span data-stu-id="e2267-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="e2267-134">[![Edição de tabelas do banco de dados](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="e2267-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="e2267-135">Criar um modelo</span><span class="sxs-lookup"><span data-stu-id="e2267-135">Creating a Model</span></span>

<span data-ttu-id="e2267-136">Agora, alterne para o Gerenciador de soluções, no lado direito do IDE e com o botão direito na pasta modelos e selecione Adicionar | Novo Item.</span><span class="sxs-lookup"><span data-stu-id="e2267-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="e2267-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="e2267-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="e2267-138">Vamos criar um modelo de entidade do nosso novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e2267-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="e2267-139">Isso adicionará um conjunto de classes ao nosso projeto que torna mais fácil para nós consultar e manipular os dados em nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e2267-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="e2267-140">Selecione o nó de dados no lado esquerdo da caixa de diálogo e, em seguida, selecione o modelo de item de modelo de dados de entidade ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="e2267-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="e2267-141">O nome Movies.edmx.</span><span class="sxs-lookup"><span data-stu-id="e2267-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="e2267-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="e2267-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="e2267-143">Clique no botão "Adicionar".</span><span class="sxs-lookup"><span data-stu-id="e2267-143">Click the "Add" button.</span></span> <span data-ttu-id="e2267-144">Isso, em seguida, iniciará o "Assistente dados de entidade modelo".</span><span class="sxs-lookup"><span data-stu-id="e2267-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="e2267-145">Na caixa de diálogo Novo pop-up, selecione Gerar do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e2267-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="e2267-146">Desde que acabou de criar um banco de dados, apenas precisamos saber o Entity Framework sobre nosso novo banco de dados e sua tabela.</span><span class="sxs-lookup"><span data-stu-id="e2267-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="e2267-147">Clique em próximo a salvar nosso conexão de banco de dados de configuração do nosso aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="e2267-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="e2267-148">Agora, verifique as tabelas e filme caixa de seleção e clique em Concluir.</span><span class="sxs-lookup"><span data-stu-id="e2267-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="e2267-149">[![Assistente de modelo de dados de entidade](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="e2267-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="e2267-150">Agora podemos ver nossa nova tabela de filme no Entity Framework Designer e acessá-lo a partir do código.</span><span class="sxs-lookup"><span data-stu-id="e2267-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="e2267-151">[![Filmes - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="e2267-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="e2267-152">Na superfície de design, você pode ver uma classe "Filme".</span><span class="sxs-lookup"><span data-stu-id="e2267-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="e2267-153">Essa classe é mapeado para a tabela "Filme" em nosso banco de dados, e cada propriedade dentro dele é mapeado para uma coluna com a tabela.</span><span class="sxs-lookup"><span data-stu-id="e2267-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="e2267-154">Cada instância de uma classe "Filme" corresponderá a uma linha na tabela "Filme".</span><span class="sxs-lookup"><span data-stu-id="e2267-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="e2267-155">Se você não gosta de nomenclatura padrão e convenções usadas pelo Entity Framework de mapeamento, você pode usar o designer do Entity Framework para alterar ou personalizá-los.</span><span class="sxs-lookup"><span data-stu-id="e2267-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="e2267-156">Para este aplicativo vamos usar os padrões e basta salvar o arquivo como-é.</span><span class="sxs-lookup"><span data-stu-id="e2267-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="e2267-157">Agora, vamos trabalhar com alguns dados reais!</span><span class="sxs-lookup"><span data-stu-id="e2267-157">Now, let's work with some real data!</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e2267-158">[Anterior](getting-started-with-mvc-part3.md)
[Próximo](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="e2267-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>

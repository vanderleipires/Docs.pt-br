---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Adicionando uma coluna para o modelo | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que leituras e gravações de banco de dados.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 7a0b64ce00fc5ee6d49990f1d4d93a154c467bf5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="0dd4a-104">Adicionando uma coluna para o modelo</span><span class="sxs-lookup"><span data-stu-id="0dd4a-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="0dd4a-105">by [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="0dd4a-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="0dd4a-106">Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="0dd4a-107">Você criará um aplicativo web simples que leituras e gravações de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="0dd4a-108">Visite o [Central de aprendizagem do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="0dd4a-109">Nesta seção, vamos percorrer como podemos fazer alterações no esquema de nosso banco de dados e controlar as alterações em nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="0dd4a-110">Vamos adicionar uma coluna "Classificação" à tabela de filme.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="0dd4a-111">Volte para o IDE e clique em Gerenciador de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="0dd4a-112">A tabela de filme clique com botão direito e selecione Abrir definição de tabela.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="0dd4a-113">Adicione uma coluna de "classificação de" conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="0dd4a-114">Desde que não temos as classificações agora, a coluna pode permitir valores nulos.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="0dd4a-115">Clique em Salvar.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-115">Click Save.</span></span>

<span data-ttu-id="0dd4a-116">[![Editando a tabela de filmes](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0dd4a-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="0dd4a-117">Em seguida, retorne ao Gerenciador de soluções e abra o arquivo Movies.edmx (que está na pasta \Models).</span><span class="sxs-lookup"><span data-stu-id="0dd4a-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="0dd4a-118">Clique com o botão direito na superfície de design (na área branca) e selecione o modelo de atualização de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="0dd4a-119">[![Filmes - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="0dd4a-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="0dd4a-120">Isso iniciará o Assistente de atualização"".</span><span class="sxs-lookup"><span data-stu-id="0dd4a-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="0dd4a-121">Clique na guia de atualização nele e clique em Concluir.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="0dd4a-122">Nossa classe de modelo do filme será atualizado com a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-122">Our Movie model class will then be updated with the new column.</span></span>

![Assistente de atualização (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="0dd4a-124">Depois de clicar em Concluir, você pode ver que a nova coluna de classificação foi adicionada à entidade filme em nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="0dd4a-125">[![Entidade de filme](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="0dd4a-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="0dd4a-126">Adicionamos uma coluna no modelo de banco de dados, mas os modos de exibição não souber sobre ele.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="0dd4a-127">Atualizar modos de exibição com as alterações do modelo</span><span class="sxs-lookup"><span data-stu-id="0dd4a-127">Update Views with Model Changes</span></span>

<span data-ttu-id="0dd4a-128">Há algumas maneiras pôde atualizamos nossos modelos de exibição para refletir a nova coluna de classificação.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="0dd4a-129">Desde que criamos esses modos de exibição por gerá-los por meio da caixa de diálogo Adicionar modo de exibição, poderíamos excluí-los e recriá-los novamente.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="0dd4a-130">No entanto, normalmente pessoas serão já feitas modificações a seus modelos de exibição de geração de scaffolding inicial e vai querer adicionar ou excluir campos manualmente, assim como foi feito com o campo ID de criar.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="0dd4a-131">Abra o modelo de \Views\Movies\Index.aspx e adicione um &lt;th&gt;classificação&lt;/th&gt; ao cabeçalho da tabela de filme.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="0dd4a-132">Adicionei minhas após gênero.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-132">I added mine after Genre.</span></span> <span data-ttu-id="0dd4a-133">Em seguida, na mesma posição de coluna, mas mais baixo, adicione uma linha para nossa nova classificação de saída.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="0dd4a-134">Nosso modelo Index.aspx final será assim:</span><span class="sxs-lookup"><span data-stu-id="0dd4a-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="0dd4a-135">Vamos, em seguida, abra o modelo \Views\Movies\Create.aspx e adicionar um rótulo e a caixa de texto para nossa nova propriedade de classificação:</span><span class="sxs-lookup"><span data-stu-id="0dd4a-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="0dd4a-136">Nosso modelo Create.aspx final ter esta aparência e vamos alterar o título e o secundário nosso navegador &lt;h2&gt; título para algo como "Criar um filme" enquanto estamos aqui!</span><span class="sxs-lookup"><span data-stu-id="0dd4a-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="0dd4a-137">Executar seu aplicativo e agora você tem um novo campo no banco de dados foi adicionado à página de criação.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="0dd4a-138">Adicione um novo filme - dessa vez com uma classificação - e clique em criar.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="0dd4a-139">[![Criar um filme - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="0dd4a-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="0dd4a-140">Depois que você clicar em criar, é enviadas para a página de índice onde você novo filme está listado com a nova coluna de classificação no banco de dados</span><span class="sxs-lookup"><span data-stu-id="0dd4a-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="0dd4a-141">[![Lista de filmes - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="0dd4a-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="0dd4a-142">Este tutorial básico temos começar a fazer controladores, associá-los a modos de exibição e à passagem de dados embutidos.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="0dd4a-143">Em seguida, é criado e criado um banco de dados e colocar alguns dados nele.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="0dd4a-144">Podemos recuperou os dados do banco de dados e exibição em uma tabela HTML.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="0dd4a-145">Em seguida, adicionamos um formulário de criação que permitem que o usuário adicionar dados ao banco de dados-se de dentro do aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="0dd4a-146">Adicionada a validação, então feita a validação usar JavaScript do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="0dd4a-147">Por fim, podemos alterou o banco de dados para incluir uma nova coluna de dados e atualizado nossas duas páginas para criar e exibir esses dados novos.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="0dd4a-148">Agora recomendo que você para passar para nosso tutorial de nível intermediário "[repositório de música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", bem como os vários vídeos e recursos em [ https://asp.net/mvc ](https://asp.net/mvc) para saber mais sobre o ASP.NET MVC!</span><span class="sxs-lookup"><span data-stu-id="0dd4a-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="0dd4a-149">Aproveite!</span><span class="sxs-lookup"><span data-stu-id="0dd4a-149">Enjoy!</span></span>

- <span data-ttu-id="0dd4a-150">Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) e [ @shanselman ](http://twitter.com/shanselman) no Twitter.</span><span class="sxs-lookup"><span data-stu-id="0dd4a-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0dd4a-151">Anterior</span><span class="sxs-lookup"><span data-stu-id="0dd4a-151">Previous</span></span>](getting-started-with-mvc-part7.md)

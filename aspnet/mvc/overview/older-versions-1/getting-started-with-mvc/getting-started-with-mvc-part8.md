---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Adicionando uma coluna para o modelo | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que lê e grava de um banco de dados.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 241f9108394561fecb15db5b6efa2661fdb128d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361754"
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="f0ab9-104">Adicionando uma coluna para o modelo</span><span class="sxs-lookup"><span data-stu-id="f0ab9-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="f0ab9-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="f0ab9-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="f0ab9-106">Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="f0ab9-107">Você criará um aplicativo web simples que lê e grava de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="f0ab9-108">Visite o [Central de informações do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="f0ab9-109">Nesta seção, vamos examinar como podemos fazer alterações no esquema de nosso banco de dados e lidar com as alterações dentro do nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="f0ab9-110">Vamos adicionar uma coluna "Classificação" a tabela Movie.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="f0ab9-111">Volte para o IDE e clique em Gerenciador de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="f0ab9-112">Tabela Movie clique com botão direito e selecione Abrir definição de tabela.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="f0ab9-113">Adicione uma coluna de "Classificação", conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="f0ab9-114">Como não há qualquer classificações agora, a coluna pode permitir nulos.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="f0ab9-115">Clique em Salvar.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-115">Click Save.</span></span>

<span data-ttu-id="f0ab9-116">[![Edição de tabela de filmes](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f0ab9-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="f0ab9-117">Em seguida, retorne ao Gerenciador de soluções e abra o arquivo Movies.edmx (que está na pasta \Models).</span><span class="sxs-lookup"><span data-stu-id="f0ab9-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="f0ab9-118">Clique com o botão direito na superfície de design (na área branca) e selecione o modelo de atualização do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="f0ab9-119">[![Filmes - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f0ab9-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="f0ab9-120">Isso iniciará o Assistente"atualização".</span><span class="sxs-lookup"><span data-stu-id="f0ab9-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="f0ab9-121">Clique na guia de atualização dentro dele e clique em Concluir.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="f0ab9-122">Nossa classe de modelo de filme, em seguida, será atualizado com a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-122">Our Movie model class will then be updated with the new column.</span></span>

![Assistente de atualização (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="f0ab9-124">Depois de clicar em Concluir, você pode ver que a nova coluna de classificação foi adicionada à entidade em nosso modelo de filme.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="f0ab9-125">[![Entidade de filme](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="f0ab9-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="f0ab9-126">Adicionamos uma coluna no modelo de banco de dados, mas os modos de exibição não souber sobre ele.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="f0ab9-127">Modos de exibição de atualizações com as alterações do modelo</span><span class="sxs-lookup"><span data-stu-id="f0ab9-127">Update Views with Model Changes</span></span>

<span data-ttu-id="f0ab9-128">Há algumas maneiras de poderia atualizamos nossos modelos de exibição para refletir a nova coluna de classificação.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="f0ab9-129">Uma vez que criamos esses modos de exibição por meio da geração-los por meio da caixa de diálogo Adicionar modo de exibição, poderíamos excluí-los e recriá-los novamente.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="f0ab9-130">No entanto, normalmente as pessoas serão já tiver feito modificações em seus modelos de exibição da geração inicial gerado por scaffolding e vai querer adicionar ou excluir campos manualmente, exatamente como fizemos com o campo de ID para criar.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="f0ab9-131">Abra o modelo \Views\Movies\Index.aspx e adicione uma &lt;ésimo&gt;Rating&lt;/th&gt; ao cabeçalho da tabela Movie.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="f0ab9-132">Eu adicionei meu após gênero.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-132">I added mine after Genre.</span></span> <span data-ttu-id="f0ab9-133">Em seguida, na mesma posição de coluna, mas mais baixo, adicione uma linha para nossa nova classificação de saída.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="f0ab9-134">Nosso modelo aspx final terá esta aparência:</span><span class="sxs-lookup"><span data-stu-id="f0ab9-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="f0ab9-135">Vamos, em seguida, abra o modelo \Views\Movies\Create.aspx e adicione um rótulo e uma caixa de texto para nossa nova propriedade de classificação:</span><span class="sxs-lookup"><span data-stu-id="f0ab9-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="f0ab9-136">Nosso modelo aspx final se parecer com isso e vamos alterar o título e o secundário nosso navegador &lt;h2&gt; title para algo como "Criar um filme" enquanto estamos fazendo aqui!</span><span class="sxs-lookup"><span data-stu-id="f0ab9-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="f0ab9-137">Executar seu aplicativo e agora você tem um novo campo no banco de dados que foi adicionado para a página criar.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="f0ab9-138">Adicionar um novo filme – desta vez com uma classificação – e clique em criar.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="f0ab9-139">[![Criar um filme - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="f0ab9-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="f0ab9-140">Depois de clicar em criar, você é enviado para a página de índice onde você novo filme é listado com a nova coluna de classificação no banco de dados</span><span class="sxs-lookup"><span data-stu-id="f0ab9-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="f0ab9-141">[![Lista de filmes - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="f0ab9-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="f0ab9-142">Este tutorial básico te a você a começar a transformar controladores, associá-los de modos de exibição e à passagem de dados embutidos.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="f0ab9-143">Em seguida, criamos e criado um banco de dados e colocar alguns dados nele.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="f0ab9-144">Podemos recuperou os dados do banco de dados e exibida-lo em uma tabela HTML.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="f0ab9-145">Em seguida, adicionamos um formulário de criação que permitem ao usuário adicionar dados ao banco de dados em si de dentro do aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="f0ab9-146">Estamos adicionado validação e fez com que a validação usar JavaScript do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="f0ab9-147">Por fim, podemos alterado no banco de dados incluem uma nova coluna de dados e nossas duas páginas atualizadas para criar e exibir esses novos dados.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="f0ab9-148">Agora recomendo que você para passar para nosso tutorial de nível intermediário "[Store de música do MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", bem como os vários vídeos e recursos [ https://asp.net/mvc ](https://asp.net/mvc) para saber mais sobre o ASP.NET MVC!</span><span class="sxs-lookup"><span data-stu-id="f0ab9-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="f0ab9-149">Aproveite!</span><span class="sxs-lookup"><span data-stu-id="f0ab9-149">Enjoy!</span></span>

- <span data-ttu-id="f0ab9-150">Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) e [ @shanselman ](http://twitter.com/shanselman) no Twitter.</span><span class="sxs-lookup"><span data-stu-id="f0ab9-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f0ab9-151">Anterior</span><span class="sxs-lookup"><span data-stu-id="f0ab9-151">Previous</span></span>](getting-started-with-mvc-part7.md)

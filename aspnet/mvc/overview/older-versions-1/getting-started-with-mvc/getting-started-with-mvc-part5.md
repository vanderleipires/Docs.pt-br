---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Acessando dados do seu modelo de um controlador | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que lê e grava de um banco de dados.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: fe5111e2f103e9b20f27385421a1985062acaac1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371607"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="fc2c8-104">Acessando dados do seu modelo de um controlador</span><span class="sxs-lookup"><span data-stu-id="fc2c8-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="fc2c8-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="fc2c8-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="fc2c8-106">Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="fc2c8-107">Você criará um aplicativo web simples que lê e grava de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="fc2c8-108">Visite o [Central de informações do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="fc2c8-109">Nesta seção, vamos criar uma nova classe MoviesController e escrever um código que recupera os nossos dados de filme e exibe-o novamente para o navegador usando um modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="fc2c8-110">Clique com o botão direito na pasta Controllers e fazer uma nova MoviesController.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="fc2c8-111">[![Adicionar controlador](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fc2c8-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="fc2c8-112">Isso criará um novo arquivo de "MoviesController.cs" sob a nossa pasta \Controllers dentro do nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="fc2c8-113">Vamos atualizar o MovieController para recuperar a lista de filmes do nosso banco de dados recentemente populado.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="fc2c8-114">Estamos executando uma consulta LINQ para que podemos apenas recuperar filmes lançados após o segundo semestre de 1984.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="fc2c8-115">Precisaremos de um modelo de exibição para renderizar esta lista de filmes de volta, então, clique com botão direito no método e selecione Adicionar modo de exibição para criá-lo.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="fc2c8-116">Na caixa de diálogo Adicionar modo de exibição indicaremos que estamos passando uma lista&lt;Movies.Models.Movie&gt; para nosso modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="fc2c8-117">Ao contrário de tempos anteriores é usada a caixa de diálogo Adicionar modo de exibição e optar por criar um modelo de "Empty", neste momento que podemos irá indicar que queremos Visual Studio automaticamente "criar o scaffolding de" um modelo de exibição para nós com algum conteúdo padrão.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="fc2c8-118">Vamos fazer isso selecionando o item "List" em "Exibir conteúdo menu suspenso.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="fc2c8-119">Lembre-se de que quando você criou uma nova classe, você precisará compilar seu aplicativo para que ele apareça na caixa de diálogo Adicionar modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Adicionar modo de exibição](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="fc2c8-121">Clique em Adicionar e o sistema gerará automaticamente o código para um modo de exibição para nós que exibe nossa lista de filmes.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="fc2c8-122">Isso é um bom momento para alterar o &lt;h2&gt; título para algo como "Minha lista de filmes" como fizemos anteriormente com a exibição de Hello World.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="fc2c8-123">[![Filmes - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="fc2c8-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="fc2c8-124">Executar o aplicativo e visite /Movies na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="fc2c8-125">Agora nós dados recuperados do banco de dados usando uma consulta básica do controlador e os dados retornados a uma exibição que sabe sobre filmes.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="fc2c8-126">Esse modo de exibição gira através da lista de filmes e cria uma tabela de dados para nós.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="fc2c8-127">[![Lista de filmes - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="fc2c8-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="fc2c8-128">Nós não implementará funcionalidade de edição, detalhes e exclusão com este aplicativo - portanto, não precisamos de links padrão que o modelo de scaffold criado para nós.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="fc2c8-129">Abra o arquivo /Movies/Index.aspx e removê-los.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="fc2c8-130">Aqui está o código-fonte para o nosso modelo de exibição atualizado aparência depois de fazermos a essas alterações:</span><span class="sxs-lookup"><span data-stu-id="fc2c8-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="fc2c8-131">Criação de links que não precisamos, portanto, nós os excluiremos para este exemplo.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="fc2c8-132">Vamos manter nosso criar novo link no entanto, como o que vem a seguir!</span><span class="sxs-lookup"><span data-stu-id="fc2c8-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="fc2c8-133">Aqui está a aparência de nosso aplicativo com aquela coluna removida.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="fc2c8-134">[![Lista de filmes - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="fc2c8-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="fc2c8-135">Agora temos uma lista simples de nossos dados de filme.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="fc2c8-136">No entanto, se podemos clicar no link "Criar novo", obterá um erro pois ele não está associado!</span><span class="sxs-lookup"><span data-stu-id="fc2c8-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="fc2c8-137">Vamos implementar um método de ação de criar e habilitar um usuário a inserir novos filmes em nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fc2c8-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fc2c8-138">[Anterior](getting-started-with-mvc-part4.md)
> [Próximo](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="fc2c8-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>

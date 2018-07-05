---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Adicionando um método de criação e criar modo de exibição | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que lê e grava de um banco de dados.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 976df78ea22c30c094f70a57792d287f15d2c62d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400902"
---
<a name="adding-a-create-method-and-create-view"></a><span data-ttu-id="fc1f1-104">Adicionando um método de criação e criar modo de exibição</span><span class="sxs-lookup"><span data-stu-id="fc1f1-104">Adding a Create Method and Create View</span></span>
====================
<span data-ttu-id="fc1f1-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="fc1f1-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="fc1f1-106">Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="fc1f1-107">Você criará um aplicativo web simples que lê e grava de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="fc1f1-108">Visite o [Central de informações do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="fc1f1-109">Nesta seção, vamos implementar o suporte necessário para permitir que os usuários criem novos filmes em nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-109">In this section we are going to implement the support necessary to enable users to create new movies in our database.</span></span> <span data-ttu-id="fc1f1-110">Faremos isso Implementando a ação de URL Movies/Create.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-110">We'll do this by implementing the /Movies/Create URL action.</span></span>

<span data-ttu-id="fc1f1-111">Implementando a URL de filmes/Criar é um processo de duas etapas.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-111">Implementing the /Movies/Create URL is a two step process.</span></span> <span data-ttu-id="fc1f1-112">Quando um usuário pela primeira vez visita a URL de filmes/Criar queremos mostrá-los em um formulário HTML que pode preencher a inserir um novo filme.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-112">When a user first visits the /Movies/Create URL we want to show them an HTML form that they can fill out to enter a new movie.</span></span> <span data-ttu-id="fc1f1-113">Em seguida, quando o usuário envia o formulário e os dados de volta para o servidor de postagens, queremos recuperar o conteúdo postado e salvá-lo em nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-113">Then, when the user submits the form and posts the data back to the server, we want to retrieve the posted contents and save it into our database.</span></span>

<span data-ttu-id="fc1f1-114">Implementaremos essas duas etapas dentro de dois métodos Create () dentro de nossa classe MoviesController.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-114">We'll implement these two steps within two Create() methods within our MoviesController class.</span></span> <span data-ttu-id="fc1f1-115">Um método mostrará a &lt;formulário&gt; que o usuário deve preencher para criar um novo filme.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-115">One method will show the &lt;form&gt; that the user should fill out to create a new movie.</span></span> <span data-ttu-id="fc1f1-116">O segundo método tratará o processamento de dados postados quando o usuário envia o &lt;formulário&gt; de volta para o servidor e salvar um novo filme dentro do nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-116">The second method will handle processing the posted data when the user submits the &lt;form&gt; back to the server, and save a new Movie within our database.</span></span>

<span data-ttu-id="fc1f1-117">Abaixo está o código, adicionaremos à nossa classe MoviesController implementar isso:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-117">Below is the code we'll add to our MoviesController class to implement this:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

<span data-ttu-id="fc1f1-118">O código acima contém todo o código que vamos precisar dentro do nosso controlador.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-118">The above code contains all of the code that we'll need within our Controller.</span></span>

<span data-ttu-id="fc1f1-119">Agora, vamos implementar o modelo de Create View que usaremos para exibir um formulário para o usuário.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-119">Let's now implement the Create View template that we'll use to display a form to the user.</span></span> <span data-ttu-id="fc1f1-120">Vamos clique com botão direito no primeiro método de criar e selecione "Adicionar exibição" para criar o modelo de exibição para nosso formulário de filme.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-120">We'll right click in the first Create method and select "Add View" to create the view template for our Movie form.</span></span>

<span data-ttu-id="fc1f1-121">Vamos selecionar que estamos indo para passar o modelo de exibição "Filme" como sua classe de dados de exibição e indicar que desejamos criar o "scaffolding" de um modelo de "Criar".</span><span class="sxs-lookup"><span data-stu-id="fc1f1-121">We'll select that we are going to pass the view template a "Movie" as its view data class, and indicate that we want to "scaffold" a "Create" template.</span></span>

<span data-ttu-id="fc1f1-122">[![Adicionar modo de exibição](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fc1f1-122">[![Add View](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)</span></span>

<span data-ttu-id="fc1f1-123">Depois de clicar no botão Add, o modelo de exibição \Movies\Create.aspx será criado para você.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-123">After you click the Add button, \Movies\Create.aspx View template will be created for you.</span></span> <span data-ttu-id="fc1f1-124">Porque nós selecionamos "Criar" na lista suspensa "modo de exibição de conteúdo", a caixa de diálogo Adicionar modo de exibição automaticamente "geradas por scaffolding" algum conteúdo padrão para nós.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-124">Because we selected "Create" from the "view content" dropdown, the Add View dialog automatically "scaffolded" some default content for us.</span></span> <span data-ttu-id="fc1f1-125">O scaffolding criou um HTML &lt;formulário&gt;, mensagens de um local para o erro de validação para ir e uma vez que o scaffolding sabe sobre filmes, ele criou o rótulo e campos para cada propriedade de nossa classe.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-125">The scaffolding created an HTML &lt;form&gt;, a place for validation error messages to go, and since scaffolding knows about Movies, it created Label and Fields for each property of our class.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

<span data-ttu-id="fc1f1-126">Como nosso banco de dados fornece automaticamente um filme uma ID, vamos remover esses campos desse modelo de referência. ID de nosso Create View.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-126">Since our database automatically gives a Movie an ID, let's remove those fields that reference model.Id from our Create View.</span></span> <span data-ttu-id="fc1f1-127">Remover as 7 linhas depois &lt;legenda&gt;campos&lt;/legend&gt; conforme eles mostram o campo de ID que não queremos.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-127">Remove the 7 lines after &lt;legend&gt;Fields&lt;/legend&gt; as they show the ID field that we don't want.</span></span>

<span data-ttu-id="fc1f1-128">Vamos agora criar um novo filme e adicioná-lo ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-128">Let's now create a new movie and add it to the database.</span></span> <span data-ttu-id="fc1f1-129">Vamos fazer isso executando o aplicativo novamente e visite a "/ filmes" URL e clique em "Criar" link para adicionar um novo filme.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-129">We'll do this by running the application again and visit the "/Movies" URL and click the "Create" link to add a new Movie.</span></span>

<span data-ttu-id="fc1f1-130">[![Criar – Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="fc1f1-130">[![Create - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)</span></span>

<span data-ttu-id="fc1f1-131">Quando clicamos no botão Criar, publicaremos novamente (por meio de HTTP POST) os dados neste formulário para o método /Movies/Create que acabamos de criar.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-131">When we click the Create button, we'll be posting back (via HTTP POST) the data on this form to the /Movies/Create method that we just created.</span></span> <span data-ttu-id="fc1f1-132">Assim como quando o sistema automaticamente o parâmetro "numTimes" e "nome" da URL e mapeados-los para parâmetros em um método anteriormente, o sistema automaticamente tirar os campos de formulário de uma POSTAGEM e mapeá-los para um objeto.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-132">Just like when the system automatically took the "numTimes" and "name " parameter out of the URL and mapped them to parameters on a method earlier, the system will automatically take the Form Fields from a POST and map them to an object.</span></span> <span data-ttu-id="fc1f1-133">Nesse caso, os valores dos campos em HTML, como "ReleaseDate" e "Title" serão automaticamente colocados nas propriedades corretas de uma nova instância de um filme.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-133">In this case, values from fields in HTML like "ReleaseDate" and "Title" will automatically be put into the correct properties of a new instance of a Movie.</span></span>

<span data-ttu-id="fc1f1-134">Vamos examinar o segundo método de criação de nosso MoviesController novamente.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-134">Let's look at the second Create method from our MoviesController again.</span></span> <span data-ttu-id="fc1f1-135">Observe como ele utiliza um objeto de "Filme" como um argumento:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-135">Notice how it takes a "Movie" object as an argument:</span></span>

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

<span data-ttu-id="fc1f1-136">Esse objeto de filme, em seguida, foi passado para a versão de [HttpPost] do nosso método de ação de criar e, ele será salvo no banco de dados e redirecionado, em seguida, o usuário para o método de ação Index () que mostra o resultado salvo na lista de filmes:</span><span class="sxs-lookup"><span data-stu-id="fc1f1-136">This Movie object was then passed to the [HttpPost] version of our Create action method, and we saved it in the database and then redirected the user back to the Index() action method which will show the saved result in the movie list:</span></span>

<span data-ttu-id="fc1f1-137">[![Lista de filmes - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="fc1f1-137">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)</span></span>

<span data-ttu-id="fc1f1-138">Nós não estão verificando se o nosso filmes estão corretos, no entanto, e o banco de dados não nos permite salvar um filme sem título.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-138">We aren't checking if our movies are correct, though, and the database won't allow us to save a movie with no Title.</span></span> <span data-ttu-id="fc1f1-139">Seria bom se poderíamos dar ao usuário que gerou um erro antes do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-139">It'd be nice if we could tell the user that before the database threw an error.</span></span> <span data-ttu-id="fc1f1-140">Faremos isso em seguida, adicionando suporte de validação ao nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fc1f1-140">We'll do this next by adding validation support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fc1f1-141">[Anterior](getting-started-with-mvc-part5.md)
> [Próximo](getting-started-with-mvc-part7.md)</span><span class="sxs-lookup"><span data-stu-id="fc1f1-141">[Previous](getting-started-with-mvc-part5.md)
[Next](getting-started-with-mvc-part7.md)</span></span>

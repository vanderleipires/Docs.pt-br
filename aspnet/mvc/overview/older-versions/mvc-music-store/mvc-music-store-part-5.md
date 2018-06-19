---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Parte 5: Formulários de edição e modelagem | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 5 abrange formulários de edição e modelagem.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: d584e614b5a4124044cd9decd2272192ca164643
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874906"
---
<a name="part-5-edit-forms-and-templating"></a><span data-ttu-id="c6eff-104">Parte 5: Formulários de edição e modelagem</span><span class="sxs-lookup"><span data-stu-id="c6eff-104">Part 5: Edit Forms and Templating</span></span>
====================
<span data-ttu-id="c6eff-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="c6eff-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="c6eff-106">O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.</span><span class="sxs-lookup"><span data-stu-id="c6eff-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="c6eff-107">O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="c6eff-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="c6eff-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c6eff-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="c6eff-109">Parte 5 abrange formulários de edição e modelagem.</span><span class="sxs-lookup"><span data-stu-id="c6eff-109">Part 5 covers Edit Forms and Templating.</span></span>


<span data-ttu-id="c6eff-110">O capítulo anterior, nós foram carregamento de dados de nosso banco de dados e exibi-lo.</span><span class="sxs-lookup"><span data-stu-id="c6eff-110">In the past chapter, we were loading data from our database and displaying it.</span></span> <span data-ttu-id="c6eff-111">Neste capítulo, também deverá habilitar edição dos dados.</span><span class="sxs-lookup"><span data-stu-id="c6eff-111">In this chapter, we'll also enable editing the data.</span></span>

## <a name="creating-the-storemanagercontroller"></a><span data-ttu-id="c6eff-112">Criando o StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="c6eff-112">Creating the StoreManagerController</span></span>

<span data-ttu-id="c6eff-113">Vamos começar criando um novo controlador chamado **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="c6eff-113">We'll begin by creating a new controller called **StoreManagerController**.</span></span> <span data-ttu-id="c6eff-114">Para este controlador, faremos aproveitar os recursos de Scaffolding disponíveis na atualização das ferramentas do ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="c6eff-114">For this controller, we will be taking advantage of the Scaffolding features available in the ASP.NET MVC 3 Tools Update.</span></span> <span data-ttu-id="c6eff-115">Defina as opções para a caixa de diálogo Adicionar controlador, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="c6eff-115">Set the options for the Add Controller dialog as shown below.</span></span>

![](mvc-music-store-part-5/_static/image1.png)

<span data-ttu-id="c6eff-116">Quando você clica no botão Adicionar, você verá que o mecanismo do ASP.NET MVC 3 scaffolding faz uma boa quantidade de trabalho para você:</span><span class="sxs-lookup"><span data-stu-id="c6eff-116">When you click the Add button, you'll see that the ASP.NET MVC 3 scaffolding mechanism does a good amount of work for you:</span></span>

- <span data-ttu-id="c6eff-117">Cria o novo StoreManagerController com uma variável local do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="c6eff-117">It creates the new StoreManagerController with a local Entity Framework variable</span></span>
- <span data-ttu-id="c6eff-118">Ele adiciona uma pasta StoreManager para a pasta de modos de exibição do projeto</span><span class="sxs-lookup"><span data-stu-id="c6eff-118">It adds a StoreManager folder to the project's Views folder</span></span>
- <span data-ttu-id="c6eff-119">Ele adiciona exibição Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml e cshtml, fortemente tipada para a classe álbum</span><span class="sxs-lookup"><span data-stu-id="c6eff-119">It adds Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml, and Index.cshtml view, strongly typed to the Album class</span></span>

![](mvc-music-store-part-5/_static/image2.png)

<span data-ttu-id="c6eff-120">A nova classe de controlador StoreManager inclui CRUD (criar, ler, atualizar e excluir) ações do controlador que sabem como trabalhar com o álbum de classe de modelo e usar o nosso contexto do Entity Framework para acesso ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c6eff-120">The new StoreManager controller class includes CRUD (create, read, update, delete) controller actions which know how to work with the Album model class and use our Entity Framework context for database access.</span></span>

## <a name="modifying-a-scaffolded-view"></a><span data-ttu-id="c6eff-121">Modificação de uma exibição de scaffolding</span><span class="sxs-lookup"><span data-stu-id="c6eff-121">Modifying a Scaffolded View</span></span>

<span data-ttu-id="c6eff-122">É importante lembrar-se de que, embora este código foi gerado para nós, é código padrão do ASP.NET MVC, assim como o você escreve em todo este tutorial.</span><span class="sxs-lookup"><span data-stu-id="c6eff-122">It's important to remember that, while this code was generated for us, it's standard ASP.NET MVC code, just like we've been writing throughout this tutorial.</span></span> <span data-ttu-id="c6eff-123">Ele tem se destina a economizar tempo você levaria em escrever o código do controlador clichê e criar os modos de exibição fortemente tipados manualmente, mas isso não é do tipo de código gerado que você pode ter visto precedido de avisos terríveis nos comentários sobre como você não deve alterar o código.</span><span class="sxs-lookup"><span data-stu-id="c6eff-123">It's intended to save you the time you'd spend on writing boilerplate controller code and creating the strongly typed views manually, but this isn't the kind of generated code you may have seen prefaced with dire warnings in comments about how you mustn't change the code.</span></span> <span data-ttu-id="c6eff-124">Este é seu código, e você deverá alterá-lo.</span><span class="sxs-lookup"><span data-stu-id="c6eff-124">This is your code, and you're expected to change it.</span></span>

<span data-ttu-id="c6eff-125">Portanto, vamos começar com uma edição rápida para o modo de exibição do índice StoreManager (/ Views/StoreManager/Index.cshtml).</span><span class="sxs-lookup"><span data-stu-id="c6eff-125">So, let's start with a quick edit to the StoreManager Index view (/Views/StoreManager/Index.cshtml).</span></span> <span data-ttu-id="c6eff-126">Essa exibição mostrará uma tabela que lista os álbuns em nosso repositório com editar / detalhes / excluir links e inclui as propriedades públicas do álbum.</span><span class="sxs-lookup"><span data-stu-id="c6eff-126">This view will display a table which lists the Albums in our store with Edit / Details / Delete links, and includes the Album's public properties.</span></span> <span data-ttu-id="c6eff-127">Vamos removê campo AlbumArtUrl, pois não é muito útil nessa exibição.</span><span class="sxs-lookup"><span data-stu-id="c6eff-127">We'll remove the AlbumArtUrl field, as it's not very useful in this display.</span></span> <span data-ttu-id="c6eff-128">Em &lt;tabela&gt; seção do código de exibição, remova o &lt;th&gt; e &lt;td&gt; elementos ao redor AlbumArtUrl referências, como indicado pelas linhas realçadas abaixo:</span><span class="sxs-lookup"><span data-stu-id="c6eff-128">In &lt;table&gt; section of the view code, remove the &lt;th&gt; and &lt;td&gt; elements surrounding AlbumArtUrl references, as indicated by the highlighted lines below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

<span data-ttu-id="c6eff-129">O código de modo de exibição modificado será exibida da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c6eff-129">The modified view code will appear as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a><span data-ttu-id="c6eff-130">Um primeiro olhar o Gerenciador de armazenamento</span><span class="sxs-lookup"><span data-stu-id="c6eff-130">A first look at the Store Manager</span></span>

<span data-ttu-id="c6eff-131">Agora execute o aplicativo e navegue até/StoreManager /.</span><span class="sxs-lookup"><span data-stu-id="c6eff-131">Now run the application and browse to /StoreManager/.</span></span> <span data-ttu-id="c6eff-132">Isso exibe apenas modificamos, mostrando uma lista dos álbuns no repositório com links para obter detalhes, editar e excluir o índice de Gerenciador de repositório.</span><span class="sxs-lookup"><span data-stu-id="c6eff-132">This displays the Store Manager Index we just modified, showing a list of the albums in the store with links to Edit, Details, and Delete.</span></span>

![](mvc-music-store-part-5/_static/image3.png)

<span data-ttu-id="c6eff-133">Clicar no link de edição exibe um formulário de edição com campos por álbum, incluindo listas suspensas para gênero e artista.</span><span class="sxs-lookup"><span data-stu-id="c6eff-133">Clicking the Edit link displays an edit form with fields for the Album, including dropdowns for Genre and Artist.</span></span>

![](mvc-music-store-part-5/_static/image4.png)

<span data-ttu-id="c6eff-134">Clique no link "Voltar à lista" na parte inferior, clique no link de detalhes para um álbum.</span><span class="sxs-lookup"><span data-stu-id="c6eff-134">Click the "Back to List" link at the bottom, then click on the Details link for an Album.</span></span> <span data-ttu-id="c6eff-135">Isso exibe informações detalhadas sobre um álbum individual.</span><span class="sxs-lookup"><span data-stu-id="c6eff-135">This displays the detail information for an individual Album.</span></span>

![](mvc-music-store-part-5/_static/image5.png)

<span data-ttu-id="c6eff-136">Novamente, clique em Voltar para o link de lista, clique no link excluir.</span><span class="sxs-lookup"><span data-stu-id="c6eff-136">Again, click the Back to List link, then click on a Delete link.</span></span> <span data-ttu-id="c6eff-137">Isso exibe uma caixa de diálogo de confirmação, mostrando os detalhes do álbum e perguntando se podemos tem certeza de que deseja excluí-lo.</span><span class="sxs-lookup"><span data-stu-id="c6eff-137">This displays a confirmation dialog, showing the album details and asking if we're sure we want to delete it.</span></span>

![](mvc-music-store-part-5/_static/image6.png)

<span data-ttu-id="c6eff-138">Clicando no botão Excluir na parte inferior excluirá o álbum e você retorna à página de índice, que mostra o álbum excluído.</span><span class="sxs-lookup"><span data-stu-id="c6eff-138">Clicking the Delete button at the bottom will delete the album and return you to the Index page, which shows the album deleted.</span></span>

<span data-ttu-id="c6eff-139">Não terminamos com o Gerenciador de armazenamento, mas é necessário trabalhar controlador e o código de exibição para as operações CRUD Iniciar em.</span><span class="sxs-lookup"><span data-stu-id="c6eff-139">We're not done with the Store Manager, but we have working controller and view code for the CRUD operations to start from.</span></span>

## <a name="looking-at-the-store-manager-controller-code"></a><span data-ttu-id="c6eff-140">Examinar o código do controlador do Gerenciador de armazenamento</span><span class="sxs-lookup"><span data-stu-id="c6eff-140">Looking at the Store Manager Controller code</span></span>

<span data-ttu-id="c6eff-141">O controlador do Gerenciador de armazenamento contém uma boa quantidade de código.</span><span class="sxs-lookup"><span data-stu-id="c6eff-141">The Store Manager Controller contains a good amount of code.</span></span> <span data-ttu-id="c6eff-142">Vamos por meio de cima para baixo.</span><span class="sxs-lookup"><span data-stu-id="c6eff-142">Let's go through this from top to bottom.</span></span> <span data-ttu-id="c6eff-143">O controlador inclui alguns namespaces padrão para um controlador MVC, bem como uma referência ao namespace nossos modelos.</span><span class="sxs-lookup"><span data-stu-id="c6eff-143">The controller includes some standard namespaces for an MVC controller, as well as a reference to our Models namespace.</span></span> <span data-ttu-id="c6eff-144">O controlador tem uma instância particular da MusicStoreEntities, usado por cada uma das ações do controlador para acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="c6eff-144">The controller has a private instance of MusicStoreEntities, used by each of the controller actions for data access.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a><span data-ttu-id="c6eff-145">Armazenar ações do Gerenciador de índice e detalhes</span><span class="sxs-lookup"><span data-stu-id="c6eff-145">Store Manager Index and Details actions</span></span>

<span data-ttu-id="c6eff-146">A exibição índice recupera uma lista de álbuns, inclusive do cada álbum referenciadas gênero artista informações e, como visto anteriormente ao trabalhar no método Store procurar.</span><span class="sxs-lookup"><span data-stu-id="c6eff-146">The index view retrieves a list of Albums, including each album's referenced Genre and Artist information, as we previously saw when working on the Store Browse method.</span></span> <span data-ttu-id="c6eff-147">O modo de exibição do índice está seguindo as referências a objetos vinculados para que ele possa exibir cada álbum gênero e artista nome, para que o controlador está sendo eficiente e consultar para obter essas informações na solicitação original.</span><span class="sxs-lookup"><span data-stu-id="c6eff-147">The Index view is following the references to the linked objects so that it can display each album's Genre name and Artist name, so the controller is being efficient and querying for this information in the original request.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

<span data-ttu-id="c6eff-148">Ação de controlador de detalhes do controlador StoreManager funciona exatamente como a ação de detalhes do controlador de armazenamento escrevemos anteriormente - ele consulta para o álbum por ID usando o método Find(), em seguida, retorna ao modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="c6eff-148">The StoreManager Controller's Details controller action works exactly the same as the Store Controller Details action we wrote previously - it queries for the Album by ID using the Find() method, then returns it to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a><span data-ttu-id="c6eff-149">A criar métodos de ação</span><span class="sxs-lookup"><span data-stu-id="c6eff-149">The Create Action Methods</span></span>

<span data-ttu-id="c6eff-150">Os métodos de ação Criar são um pouco diferentes daquelas que vimos até agora, porque eles tratam de entrada de formulário.</span><span class="sxs-lookup"><span data-stu-id="c6eff-150">The Create action methods are a little different from ones we've seen so far, because they handle form input.</span></span> <span data-ttu-id="c6eff-151">Quando um usuário acessa primeiro /StoreManager/criação/eles serão mostrados um formulário vazio.</span><span class="sxs-lookup"><span data-stu-id="c6eff-151">When a user first visits /StoreManager/Create/ they will be shown an empty form.</span></span> <span data-ttu-id="c6eff-152">Esta página HTML conterá um &lt;formulário&gt; onde eles podem digitar os detalhes do álbum de elementos de entrada do elemento que contém a lista suspensa e caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="c6eff-152">This HTML page will contain a &lt;form&gt; element that contains dropdown and textbox input elements where they can enter the album's details.</span></span>

<span data-ttu-id="c6eff-153">Depois que o usuário preenche os valores de formulário álbum, pressione o botão "Salvar" para enviar que essas alterações de volta para o nosso aplicativo ao salvar no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c6eff-153">After the user fills in the Album form values, they can press the "Save" button to submit these changes back to our application to save within the database.</span></span> <span data-ttu-id="c6eff-154">Quando o usuário pressiona o botão "Salvar" o &lt;formulário&gt; executará um HTTP POST para a URL de /StoreManager/criação/e enviar o &lt;formulário&gt; valores como parte do POST HTTP.</span><span class="sxs-lookup"><span data-stu-id="c6eff-154">When the user presses the "save" button the &lt;form&gt; will perform an HTTP-POST back to the /StoreManager/Create/ URL and submit the &lt;form&gt; values as part of the HTTP-POST.</span></span>

<span data-ttu-id="c6eff-155">ASP.NET MVC permite dividir facilmente a lógica desses dois cenários de invocação de URL, permitindo que nós a implementação de dois métodos de ação "Criar" separados em nossa classe StoreManagerController – um para tratar de HTTP GET inicial, navegue para o /StoreManager/Create / URL e a outra para manipular o HTTP POST das alterações enviadas.</span><span class="sxs-lookup"><span data-stu-id="c6eff-155">ASP.NET MVC allows us to easily split up the logic of these two URL invocation scenarios by enabling us to implement two separate "Create" action methods within our StoreManagerController class – one to handle the initial HTTP-GET browse to the /StoreManager/Create/ URL, and the other to handle the HTTP-POST of the submitted changes.</span></span>

### <a name="passing-information-to-a-view-using-viewbag"></a><span data-ttu-id="c6eff-156">Passar informações para uma exibição usando ViewBag</span><span class="sxs-lookup"><span data-stu-id="c6eff-156">Passing information to a View using ViewBag</span></span>

<span data-ttu-id="c6eff-157">Usei a ViewBag anteriormente neste tutorial, mas não falamos muito sobre ele.</span><span class="sxs-lookup"><span data-stu-id="c6eff-157">We've used the ViewBag earlier in this tutorial, but haven't talked much about it.</span></span> <span data-ttu-id="c6eff-158">A ViewBag nos permite passar informações para o modo de exibição sem usar um objeto de modelo com rigidez de tipos.</span><span class="sxs-lookup"><span data-stu-id="c6eff-158">The ViewBag allows us to pass information to the view without using a strongly typed model object.</span></span> <span data-ttu-id="c6eff-159">Nesse caso, nossa ação do controlador Editar HTTP GET precisa passar uma lista de artistas e gêneros para o formulário para preencher as listas suspensas e a maneira mais simples de fazer isso é para retorná-los como itens ViewBag.</span><span class="sxs-lookup"><span data-stu-id="c6eff-159">In this case, our Edit HTTP-GET controller action needs to pass both a list of Genres and Artists to the form to populate the dropdowns, and the simplest way to do that is to return them as ViewBag items.</span></span>

<span data-ttu-id="c6eff-160">A ViewBag é um objeto dinâmico, o que significa que você pode digitar ViewBag.Foo ou ViewBag.YourNameHere sem escrever código para definir essas propriedades.</span><span class="sxs-lookup"><span data-stu-id="c6eff-160">The ViewBag is a dynamic object, meaning that you can type ViewBag.Foo or ViewBag.YourNameHere without writing code to define those properties.</span></span> <span data-ttu-id="c6eff-161">Nesse caso, o código do controlador usa ViewBag.GenreId e ViewBag.ArtistId para que os valores de lista suspensa enviados com o formulário será GenreId e ArtistId, que são as propriedades de álbum estiver configurando.</span><span class="sxs-lookup"><span data-stu-id="c6eff-161">In this case, the controller code uses ViewBag.GenreId and ViewBag.ArtistId so that the dropdown values submitted with the form will be GenreId and ArtistId, which are the Album properties they will be setting.</span></span>

<span data-ttu-id="c6eff-162">Esses valores de lista suspensa são retornados para o formulário usando o objeto SelectList, que é criado apenas para essa finalidade.</span><span class="sxs-lookup"><span data-stu-id="c6eff-162">These dropdown values are returned to the form using the SelectList object, which is built just for that purpose.</span></span> <span data-ttu-id="c6eff-163">Isso é feito usando código como este:</span><span class="sxs-lookup"><span data-stu-id="c6eff-163">This is done using code like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

<span data-ttu-id="c6eff-164">Como você pode ver do código do método de ação, três parâmetros são sendo usados para criar este objeto:</span><span class="sxs-lookup"><span data-stu-id="c6eff-164">As you can see from the action method code, three parameters are being used to create this object:</span></span>

- <span data-ttu-id="c6eff-165">A lista de itens que deseja exibir a lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="c6eff-165">The list of items the dropdown will be displaying.</span></span> <span data-ttu-id="c6eff-166">Observe que isso não é apenas uma cadeia de caracteres – estamos passando uma lista de gêneros.</span><span class="sxs-lookup"><span data-stu-id="c6eff-166">Note that this isn't just a string - we're passing a list of Genres.</span></span>
- <span data-ttu-id="c6eff-167">O próximo parâmetro que está sendo passado para o SelectList é o valor selecionado.</span><span class="sxs-lookup"><span data-stu-id="c6eff-167">The next parameter being passed to the SelectList is the Selected Value.</span></span> <span data-ttu-id="c6eff-168">Este como o SelectList sabe como pre-selecionar um item na lista.</span><span class="sxs-lookup"><span data-stu-id="c6eff-168">This how the SelectList knows how to pre-select an item in the list.</span></span> <span data-ttu-id="c6eff-169">Isso será mais fácil de entender quando vamos examinar o formulário de edição, o que é bastante semelhante.</span><span class="sxs-lookup"><span data-stu-id="c6eff-169">This will be easier to understand when we look at the Edit form, which is pretty similar.</span></span>
- <span data-ttu-id="c6eff-170">O parâmetro final é a propriedade a ser exibida.</span><span class="sxs-lookup"><span data-stu-id="c6eff-170">The final parameter is the property to be displayed.</span></span> <span data-ttu-id="c6eff-171">Nesse caso, isso é que indica que a propriedade Genre.Name é o que será mostrado ao usuário.</span><span class="sxs-lookup"><span data-stu-id="c6eff-171">In this case, this is indicating that the Genre.Name property is what will be shown to the user.</span></span>

<span data-ttu-id="c6eff-172">Com isso em mente, em seguida, a ação Criar HTTP GET é bastante simple - dois SelectLists são adicionados a ViewBag e nenhum objeto de modelo é passado para o formulário (desde que ele não tenha sido criado).</span><span class="sxs-lookup"><span data-stu-id="c6eff-172">With that in mind, then, the HTTP-GET Create action is pretty simple - two SelectLists are added to the ViewBag, and no model object is passed to the form (since it hasn't been created yet).</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a><span data-ttu-id="c6eff-173">Auxiliares HTML para exibir a liste Drop em Create View</span><span class="sxs-lookup"><span data-stu-id="c6eff-173">HTML Helpers to display the Drop Downs in the Create View</span></span>

<span data-ttu-id="c6eff-174">Como falamos sobre como a lista suspensa de valores são passados para o modo de exibição, vamos dar uma olhada rápida no modo de exibição para ver como esses valores são exibidos.</span><span class="sxs-lookup"><span data-stu-id="c6eff-174">Since we've talked about how the drop down values are passed to the view, let's take a quick look at the view to see how those values are displayed.</span></span> <span data-ttu-id="c6eff-175">No código de exibição (/ Views/StoreManager/Create.cshtml), você verá a seguinte chamada é feita para exibir o descarte de gênero para baixo.</span><span class="sxs-lookup"><span data-stu-id="c6eff-175">In the view code (/Views/StoreManager/Create.cshtml), you'll see the following call is made to display the Genre drop down.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

<span data-ttu-id="c6eff-176">Isso é conhecido como um auxiliar HTML - um método de utilitário que executa uma tarefa comum do modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="c6eff-176">This is known as an HTML Helper - a utility method which performs a common view task.</span></span> <span data-ttu-id="c6eff-177">Auxiliares HTML são muito úteis para manter nossos Exibir código conciso e legível.</span><span class="sxs-lookup"><span data-stu-id="c6eff-177">HTML Helpers are very useful in keeping our view code concise and readable.</span></span> <span data-ttu-id="c6eff-178">O auxiliar Html.DropDownList é fornecido pelo ASP.NET MVC, mas como você verá posteriormente é possível criar nossos própria auxiliares para exibir o código que reutilizará em nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c6eff-178">The Html.DropDownList helper is provided by ASP.NET MVC, but as we'll see later it's possible to create our own helpers for view code we'll reuse in our application.</span></span>

<span data-ttu-id="c6eff-179">A chamada de Html.DropDownList só precisa ser informado que duas coisas - onde obter a lista para exibir e qual é o valor (se houver) deve ser pré-selecionada.</span><span class="sxs-lookup"><span data-stu-id="c6eff-179">The Html.DropDownList call just needs to be told two things - where to get the list to display, and what value (if any) should be pre-selected.</span></span> <span data-ttu-id="c6eff-180">O primeiro parâmetro, GenreId, informa DropDownList para procurar um valor chamado GenreId no modelo ou ViewBag.</span><span class="sxs-lookup"><span data-stu-id="c6eff-180">The first parameter, GenreId, tells the DropDownList to look for a value named GenreId in either the model or ViewBag.</span></span> <span data-ttu-id="c6eff-181">O segundo parâmetro é usado para indicar o valor para mostrar como inicialmente selecionado na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="c6eff-181">The second parameter is used to indicate the value to show as initially selected in the drop down list.</span></span> <span data-ttu-id="c6eff-182">Como este formulário é um formulário de criação, não há nenhum valor seja pré-selecionados e Empty é passado.</span><span class="sxs-lookup"><span data-stu-id="c6eff-182">Since this form is a Create form, there's no value to be preselected and String.Empty is passed.</span></span>

### <a name="handling-the-posted-form-values"></a><span data-ttu-id="c6eff-183">Tratamento de valores de formulário postado</span><span class="sxs-lookup"><span data-stu-id="c6eff-183">Handling the Posted Form values</span></span>

<span data-ttu-id="c6eff-184">Conforme abordado anteriormente, há dois métodos de ação associados a cada formulário.</span><span class="sxs-lookup"><span data-stu-id="c6eff-184">As we discussed before, there are two action methods associated with each form.</span></span> <span data-ttu-id="c6eff-185">O primeiro manipula a solicitação HTTP GET e exibe o formulário.</span><span class="sxs-lookup"><span data-stu-id="c6eff-185">The first handles the HTTP-GET request and displays the form.</span></span> <span data-ttu-id="c6eff-186">O segundo manipula a solicitação HTTP POST, que contém os valores de formulário enviado.</span><span class="sxs-lookup"><span data-stu-id="c6eff-186">The second handles the HTTP-POST request, which contains the submitted form values.</span></span> <span data-ttu-id="c6eff-187">Observe que a ação do controlador tem um atributo [HttpPost], que faz com que o ASP.NET MVC que ele só deve responder a solicitações HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="c6eff-187">Notice that controller action has an [HttpPost] attribute, which tells ASP.NET MVC that it should only respond to HTTP-POST requests.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

<span data-ttu-id="c6eff-188">Essa ação tem quatro responsabilidades:</span><span class="sxs-lookup"><span data-stu-id="c6eff-188">This action has four responsibilities:</span></span>

- 1. <span data-ttu-id="c6eff-189">Ler os valores de formulário</span><span class="sxs-lookup"><span data-stu-id="c6eff-189">Read the form values</span></span>
- 2. <span data-ttu-id="c6eff-190">Verifique se os valores de formulário passam todas as regras de validação</span><span class="sxs-lookup"><span data-stu-id="c6eff-190">Check if the form values pass any validation rules</span></span>
- 3. <span data-ttu-id="c6eff-191">Se o envio do formulário é válido, salve os dados e exibir a lista atualizada</span><span class="sxs-lookup"><span data-stu-id="c6eff-191">If the form submission is valid, save the data and display the updated list</span></span>
- 4. <span data-ttu-id="c6eff-192">Se o envio do formulário não é válido, reexiba o formulário com erros de validação</span><span class="sxs-lookup"><span data-stu-id="c6eff-192">If the form submission is not valid, redisplay the form with validation errors</span></span>

#### <a name="reading-form-values-with-model-binding"></a><span data-ttu-id="c6eff-193">Valores de formulário de leitura com associação de modelo</span><span class="sxs-lookup"><span data-stu-id="c6eff-193">Reading Form Values with Model Binding</span></span>

<span data-ttu-id="c6eff-194">Ação do controlador está processando uma submissão de formulário que inclui valores de GenreId e ArtistId (na lista suspensa) e os valores de caixa de texto de título, preço e AlbumArtUrl.</span><span class="sxs-lookup"><span data-stu-id="c6eff-194">The controller action is processing a form submission that includes values for GenreId and ArtistId (from the drop down list) and textbox values for Title, Price, and AlbumArtUrl.</span></span> <span data-ttu-id="c6eff-195">Embora seja possível acessar diretamente os valores de formulário, uma abordagem melhor é usar os recursos de associação de modelo criados no ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c6eff-195">While it's possible to directly access form values, a better approach is to use the Model Binding capabilities built into ASP.NET MVC.</span></span> <span data-ttu-id="c6eff-196">Quando uma ação do controlador tem um tipo de modelo como um parâmetro, o ASP.NET MVC tentará preencher um objeto desse tipo usando entradas de formulário (bem como valores de rota e querystring).</span><span class="sxs-lookup"><span data-stu-id="c6eff-196">When a controller action takes a model type as a parameter, ASP.NET MVC will attempt to populate an object of that type using form inputs (as well as route and querystring values).</span></span> <span data-ttu-id="c6eff-197">Isso é feito procurando valores cujos nomes correspondem a propriedades do objeto de modelo, por exemplo, ao configurar o novo álbum valor do objeto GenreId, ele procura uma entrada com o nome GenreId.</span><span class="sxs-lookup"><span data-stu-id="c6eff-197">It does this by looking for values whose names match properties of the model object, e.g. when setting the new Album object's GenreId value, it looks for an input with the name GenreId.</span></span> <span data-ttu-id="c6eff-198">Quando você criar modos de exibição usando os métodos padrão do ASP.NET MVC, os formulários sempre serão processados usando nomes de propriedades como nomes de campo de entrada, para que isso os nomes de campo apenas coincidirá.</span><span class="sxs-lookup"><span data-stu-id="c6eff-198">When you create views using the standard methods in ASP.NET MVC, the forms will always be rendered using property names as input field names, so this the field names will just match up.</span></span>

#### <a name="validating-the-model"></a><span data-ttu-id="c6eff-199">Validação do modelo</span><span class="sxs-lookup"><span data-stu-id="c6eff-199">Validating the Model</span></span>

<span data-ttu-id="c6eff-200">O modelo é validado com uma chamada simple para o ModelState.</span><span class="sxs-lookup"><span data-stu-id="c6eff-200">The model is validated with a simple call to ModelState.IsValid.</span></span> <span data-ttu-id="c6eff-201">Ainda não adicionamos quaisquer regras de validação para nossa classe álbum ainda - faremos isso um pouco - direito agora essa verificação não tem muito o que fazer.</span><span class="sxs-lookup"><span data-stu-id="c6eff-201">We haven't added any validation rules to our Album class yet - we'll do that in a bit - so right now this check doesn't have much to do.</span></span> <span data-ttu-id="c6eff-202">O importante é que essa verificação ModelStat.IsValid adaptará às regras de validação que colocamos em nosso modelo, para que as alterações futuras para regras de validação não necessitam de atualizações para o código de ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="c6eff-202">What's important is that this ModelStat.IsValid check will adapt to the validation rules we put on our model, so future changes to validation rules won't require any updates to the controller action code.</span></span>

#### <a name="saving-the-submitted-values"></a><span data-ttu-id="c6eff-203">Salvar os valores enviados</span><span class="sxs-lookup"><span data-stu-id="c6eff-203">Saving the submitted values</span></span>

<span data-ttu-id="c6eff-204">Se o envio do formulário passar na validação, é hora de salvar os valores para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c6eff-204">If the form submission passes validation, it's time to save the values to the database.</span></span> <span data-ttu-id="c6eff-205">Com o Entity Framework, que exige apenas adicionar modelo à coleção álbuns e chamar SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="c6eff-205">With Entity Framework, that just requires adding the model to the Albums collection and calling SaveChanges.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

<span data-ttu-id="c6eff-206">Entity Framework gera os comandos SQL apropriados para manter o valor.</span><span class="sxs-lookup"><span data-stu-id="c6eff-206">Entity Framework generates the appropriate SQL commands to persist the value.</span></span> <span data-ttu-id="c6eff-207">Depois de salvar os dados, podemos redirecionar voltar à lista de álbuns então podemos ver nossa atualização.</span><span class="sxs-lookup"><span data-stu-id="c6eff-207">After saving the data, we redirect back to the list of Albums so we can see our update.</span></span> <span data-ttu-id="c6eff-208">Isso é feito retornando RedirectToAction com o nome da ação do controlador que deseja exibir.</span><span class="sxs-lookup"><span data-stu-id="c6eff-208">This is done by returning RedirectToAction with the name of the controller action we want displayed.</span></span> <span data-ttu-id="c6eff-209">Nesse caso, que é o método de índice.</span><span class="sxs-lookup"><span data-stu-id="c6eff-209">In this case, that's the Index method.</span></span>

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a><span data-ttu-id="c6eff-210">Exibindo os envios de formulário inválido com erros de validação</span><span class="sxs-lookup"><span data-stu-id="c6eff-210">Displaying invalid form submissions with Validation Errors</span></span>

<span data-ttu-id="c6eff-211">No caso de entrada de forma inválida, os valores de lista suspensa são adicionados ao ViewBag (como no caso de HTTP GET) e os valores do modelo associado são retransmitidos para o modo de exibição para exibição.</span><span class="sxs-lookup"><span data-stu-id="c6eff-211">In the case of invalid form input, the dropdown values are added to the ViewBag (as in the HTTP-GET case) and the bound model values are passed back to the view for display.</span></span> <span data-ttu-id="c6eff-212">Erros de validação são exibidos automaticamente usando o @Html.ValidationMessageFor auxiliar HTML.</span><span class="sxs-lookup"><span data-stu-id="c6eff-212">Validation errors are automatically displayed using the @Html.ValidationMessageFor HTML Helper.</span></span>

#### <a name="testing-the-create-form"></a><span data-ttu-id="c6eff-213">O formulário de criação de teste</span><span class="sxs-lookup"><span data-stu-id="c6eff-213">Testing the Create Form</span></span>

<span data-ttu-id="c6eff-214">Para testar isso, execute o aplicativo e navegue até /StoreManager/criação / - Isso mostrará o formulário em branco que foi retornado pelo método StoreController criar HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="c6eff-214">To test this out, run the application and browse to /StoreManager/Create/ - this will show you the blank form which was returned by the StoreController Create HTTP-GET method.</span></span>

<span data-ttu-id="c6eff-215">Preencha alguns valores e clique no botão Criar para enviar o formulário.</span><span class="sxs-lookup"><span data-stu-id="c6eff-215">Fill in some values and click the Create button to submit the form.</span></span>

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a><span data-ttu-id="c6eff-216">Tratamento de edições</span><span class="sxs-lookup"><span data-stu-id="c6eff-216">Handling Edits</span></span>

<span data-ttu-id="c6eff-217">O par de ação de edição (HTTP GET e POST HTTP) é muito semelhante aos métodos de ação de criar que acabamos de ver.</span><span class="sxs-lookup"><span data-stu-id="c6eff-217">The Edit action pair (HTTP-GET and HTTP-POST) are very similar to the Create action methods we just looked at.</span></span> <span data-ttu-id="c6eff-218">Como o cenário de edição envolve trabalhar com um álbum existente, editar HTTP-GET método carrega o álbum com base no parâmetro "id", passado por meio de rota.</span><span class="sxs-lookup"><span data-stu-id="c6eff-218">Since the edit scenario involves working with an existing album, the Edit HTTP-GET method loads the Album based on the "id" parameter, passed in via the route.</span></span> <span data-ttu-id="c6eff-219">Esse código para recuperar um álbum por AlbumId é o mesmo anteriormente vimos na ação de controlador de detalhes.</span><span class="sxs-lookup"><span data-stu-id="c6eff-219">This code for retrieving an album by AlbumId is the same as we've previously looked at in the Details controller action.</span></span> <span data-ttu-id="c6eff-220">Assim como acontece com a criação / método HTTP GET, na lista suspensa de valores são retornados por meio de ViewBag.</span><span class="sxs-lookup"><span data-stu-id="c6eff-220">As with the Create / HTTP-GET method, the drop down values are returned via the ViewBag.</span></span> <span data-ttu-id="c6eff-221">Isso nos permite retornar um álbum como nosso objeto de modelo para o modo de exibição (que é fortemente tipado para a classe álbum) passando dados adicionais (por exemplo, uma lista de gêneros) via ViewBag.</span><span class="sxs-lookup"><span data-stu-id="c6eff-221">This allows us to return an Album as our model object to the view (which is strongly typed to the Album class) while passing additional data (e.g. a list of Genres) via the ViewBag.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

<span data-ttu-id="c6eff-222">A ação de editar HTTP POST é muito semelhante à ação Criar HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="c6eff-222">The Edit HTTP-POST action is very similar to the Create HTTP-POST action.</span></span> <span data-ttu-id="c6eff-223">A única diferença é que, em vez de adicionar um novo álbum ao banco de dados. Coleção de álbuns, podemos está localizando a instância atual do álbum usando o banco de dados. Entry(Album) e definir seu estado como modificado.</span><span class="sxs-lookup"><span data-stu-id="c6eff-223">The only difference is that instead of adding a new album to the db.Albums collection, we're finding the current instance of the Album using db.Entry(album) and setting its state to Modified.</span></span> <span data-ttu-id="c6eff-224">Isso informa o Entity Framework que estamos modificando um álbum existente em vez de criar um novo.</span><span class="sxs-lookup"><span data-stu-id="c6eff-224">This tells Entity Framework that we are modifying an existing album as opposed to creating a new one.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

<span data-ttu-id="c6eff-225">Podemos testar isso executando o aplicativo e navegando até StoreManger /, em seguida, clique no link de edição para um álbum.</span><span class="sxs-lookup"><span data-stu-id="c6eff-225">We can test this out by running the application and browsing to /StoreManger/, then clicking the Edit link for an album.</span></span>

![](mvc-music-store-part-5/_static/image9.png)

<span data-ttu-id="c6eff-226">Exibe o formulário de edição mostrado pelo método Editar HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="c6eff-226">This displays the Edit form shown by the Edit HTTP-GET method.</span></span> <span data-ttu-id="c6eff-227">Preencha alguns valores e clique no botão Salvar.</span><span class="sxs-lookup"><span data-stu-id="c6eff-227">Fill in some values and click the Save button.</span></span>

![](mvc-music-store-part-5/_static/image10.png)

<span data-ttu-id="c6eff-228">Isso envia o formulário, salva os valores e retorna à lista de álbum, mostrando que os valores foram atualizados.</span><span class="sxs-lookup"><span data-stu-id="c6eff-228">This posts the form, saves the values, and returns us to the Album list, showing that the values were updated.</span></span>

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a><span data-ttu-id="c6eff-229">Tratamento de exclusão</span><span class="sxs-lookup"><span data-stu-id="c6eff-229">Handling Deletion</span></span>

<span data-ttu-id="c6eff-230">Exclusão segue o mesmo padrão como editar e criar, usando a ação de um controlador para exibir o formulário de confirmação e outra ação de controlador para lidar com o envio do formulário.</span><span class="sxs-lookup"><span data-stu-id="c6eff-230">Deletion follows the same pattern as Edit and Create, using one controller action to display the confirmation form, and another controller action to handle the form submission.</span></span>

<span data-ttu-id="c6eff-231">A ação de controlador HTTP GET e Delete é exatamente o nosso ação de controlador de detalhes do Gerenciador de armazenamento anterior.</span><span class="sxs-lookup"><span data-stu-id="c6eff-231">The HTTP-GET Delete controller action is exactly the same as our previous Store Manager Details controller action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

<span data-ttu-id="c6eff-232">Podemos exibir um formulário que é fortemente tipado para um tipo de álbum, usando o modelo de conteúdo de modo de exibição de exclusão.</span><span class="sxs-lookup"><span data-stu-id="c6eff-232">We display a form that's strongly typed to an Album type, using the Delete view content template.</span></span>

![](mvc-music-store-part-5/_static/image12.png)

<span data-ttu-id="c6eff-233">O modelo de exclusão mostra todos os campos para o modelo, mas podemos simplificar que busca um pouco.</span><span class="sxs-lookup"><span data-stu-id="c6eff-233">The Delete template shows all the fields for the model, but we can simplify that down quite a bit.</span></span> <span data-ttu-id="c6eff-234">Altere o código de exibição no /Views/StoreManager/Delete.cshtml para o seguinte.</span><span class="sxs-lookup"><span data-stu-id="c6eff-234">Change the view code in /Views/StoreManager/Delete.cshtml to the following.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

<span data-ttu-id="c6eff-235">Isso exibirá uma confirmação de exclusão simplificada.</span><span class="sxs-lookup"><span data-stu-id="c6eff-235">This displays a simplified Delete confirmation.</span></span>

![](mvc-music-store-part-5/_static/image13.png)

<span data-ttu-id="c6eff-236">Clicando no botão Excluir faz com que o formulário a ser lançado para o servidor que executa a ação de DeleteConfirmed.</span><span class="sxs-lookup"><span data-stu-id="c6eff-236">Clicking the Delete button causes the form to be posted back to the server, which executes the DeleteConfirmed action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

<span data-ttu-id="c6eff-237">Ação do controlador excluir nosso HTTP POST executa as seguintes ações:</span><span class="sxs-lookup"><span data-stu-id="c6eff-237">Our HTTP-POST Delete Controller Action takes the following actions:</span></span>

- 1. <span data-ttu-id="c6eff-238">Carrega o álbum por ID</span><span class="sxs-lookup"><span data-stu-id="c6eff-238">Loads the Album by ID</span></span>
- 2. <span data-ttu-id="c6eff-239">Exclui o álbum e salvar as alterações</span><span class="sxs-lookup"><span data-stu-id="c6eff-239">Deletes it the album and save changes</span></span>
- 3. <span data-ttu-id="c6eff-240">Redireciona para o índice, mostrando que o álbum foi removido da lista</span><span class="sxs-lookup"><span data-stu-id="c6eff-240">Redirects to the Index, showing that the Album was removed from the list</span></span>

<span data-ttu-id="c6eff-241">Para testar isso, execute o aplicativo e navegue até /StoreManager.</span><span class="sxs-lookup"><span data-stu-id="c6eff-241">To test this, run the application and browse to /StoreManager.</span></span> <span data-ttu-id="c6eff-242">Selecione um álbum na lista e clique no link excluir.</span><span class="sxs-lookup"><span data-stu-id="c6eff-242">Select an album from the list and click the Delete link.</span></span>

![](mvc-music-store-part-5/_static/image14.png)

<span data-ttu-id="c6eff-243">Isso exibe a nossa tela de confirmação de exclusão.</span><span class="sxs-lookup"><span data-stu-id="c6eff-243">This displays our Delete confirmation screen.</span></span>

![](mvc-music-store-part-5/_static/image15.png)

<span data-ttu-id="c6eff-244">Clicando no botão Excluir remove o álbum e retorna à página de índice de Gerenciador de armazenamento, que mostra que o álbum foi excluído.</span><span class="sxs-lookup"><span data-stu-id="c6eff-244">Clicking the Delete button removes the album and returns us to the Store Manager Index page, which shows that the album has been deleted.</span></span>

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a><span data-ttu-id="c6eff-245">Usando um auxiliar HTML personalizado para truncar o texto</span><span class="sxs-lookup"><span data-stu-id="c6eff-245">Using a custom HTML Helper to truncate text</span></span>

<span data-ttu-id="c6eff-246">Temos um possível problema com nossa página de índice do Gerenciador de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="c6eff-246">We've got one potential issue with our Store Manager Index page.</span></span> <span data-ttu-id="c6eff-247">Nosso propriedades álbum e artista nome podem ambos ser suficientemente longa que eles podem acionar desativar a formatação de nossa tabela.</span><span class="sxs-lookup"><span data-stu-id="c6eff-247">Our Album Title and Artist Name properties can both be long enough that they could throw off our table formatting.</span></span> <span data-ttu-id="c6eff-248">Vamos criar um auxiliar HTML personalizado para permitir que nós truncar facilmente essas e outras propriedades no nosso modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="c6eff-248">We'll create a custom HTML Helper to allow us to easily truncate these and other properties in our Views.</span></span>

![](mvc-music-store-part-5/_static/image17.png)

<span data-ttu-id="c6eff-249">Do Razor @helper sintaxe tornou muito fácil criar suas próprias funções auxiliares para uso em exibições.</span><span class="sxs-lookup"><span data-stu-id="c6eff-249">Razor's @helper syntax has made it pretty easy to create your own helper functions for use in your views.</span></span> <span data-ttu-id="c6eff-250">Abra o modo de exibição /Views/StoreManager/Index.cshtml e adicione o código a seguir diretamente após o @model linha.</span><span class="sxs-lookup"><span data-stu-id="c6eff-250">Open the /Views/StoreManager/Index.cshtml view and add the following code directly after the @model line.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

<span data-ttu-id="c6eff-251">Esse método auxiliar usa uma cadeia de caracteres e um comprimento máximo para permitir.</span><span class="sxs-lookup"><span data-stu-id="c6eff-251">This helper method takes a string and a maximum length to allow.</span></span> <span data-ttu-id="c6eff-252">Se o texto fornecido é menor que o comprimento especificado, o auxiliar gera como-é.</span><span class="sxs-lookup"><span data-stu-id="c6eff-252">If the text supplied is shorter than the length specified, the helper outputs it as-is.</span></span> <span data-ttu-id="c6eff-253">Se for mais longo, ele truncará o texto e renderiza "..." para o restante.</span><span class="sxs-lookup"><span data-stu-id="c6eff-253">If it is longer, then it truncates the text and renders "…" for the remainder.</span></span>

<span data-ttu-id="c6eff-254">Agora podemos usar nossa Truncate auxiliar para garantir que o título do álbum e o nome de artista propriedades menos de 25 caracteres.</span><span class="sxs-lookup"><span data-stu-id="c6eff-254">Now we can use our Truncate helper to ensure that both the Album Title and Artist Name properties are less than 25 characters.</span></span> <span data-ttu-id="c6eff-255">O código de exibição completa usando nosso novo auxiliar de truncamento é exibida abaixo.</span><span class="sxs-lookup"><span data-stu-id="c6eff-255">The complete view code using our new Truncate helper appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

<span data-ttu-id="c6eff-256">Agora quando pesquisamos a URL /StoreManager/, álbuns de títulos são mantidos abaixo os comprimentos máximos.</span><span class="sxs-lookup"><span data-stu-id="c6eff-256">Now when we browse the /StoreManager/ URL, the albums and titles are kept below our maximum lengths.</span></span>

![](mvc-music-store-part-5/_static/image18.png)

<span data-ttu-id="c6eff-257">Observação: Isso mostra o caso mais simples de criar e usar um auxiliar em uma exibição.</span><span class="sxs-lookup"><span data-stu-id="c6eff-257">Note: This shows the simple case of creating and using a helper in one view.</span></span> <span data-ttu-id="c6eff-258">Para saber mais sobre a criação de auxiliares que podem ser usados em todo o site, consulte a postagem do meu blog: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span><span class="sxs-lookup"><span data-stu-id="c6eff-258">To learn more about creating helpers that you can use throughout your site, see my blog post: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="c6eff-259">[Anterior](mvc-music-store-part-4.md)
> [Próximo](mvc-music-store-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="c6eff-259">[Previous](mvc-music-store-part-4.md)
[Next](mvc-music-store-part-6.md)</span></span>

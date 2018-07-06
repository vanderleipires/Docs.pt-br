---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Parte 5: Formulários de edição e modelagem | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 5 aborda os formulários de edição e modelagem.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: f799c4d492e88f3edcf3800e66e0a1bae3845ba2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395195"
---
<a name="part-5-edit-forms-and-templating"></a><span data-ttu-id="a96c4-104">Parte 5: Formulários de edição e modelagem</span><span class="sxs-lookup"><span data-stu-id="a96c4-104">Part 5: Edit Forms and Templating</span></span>
====================
<span data-ttu-id="a96c4-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="a96c4-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="a96c4-106">A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.</span><span class="sxs-lookup"><span data-stu-id="a96c4-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="a96c4-107">A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="a96c4-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="a96c4-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a96c4-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="a96c4-109">Parte 5 aborda os formulários de edição e modelagem.</span><span class="sxs-lookup"><span data-stu-id="a96c4-109">Part 5 covers Edit Forms and Templating.</span></span>


<span data-ttu-id="a96c4-110">No capítulo anterior, estávamos Carregando dados de nosso banco de dados e exibi-la.</span><span class="sxs-lookup"><span data-stu-id="a96c4-110">In the past chapter, we were loading data from our database and displaying it.</span></span> <span data-ttu-id="a96c4-111">Neste capítulo, também será habilitar edição de dados.</span><span class="sxs-lookup"><span data-stu-id="a96c4-111">In this chapter, we'll also enable editing the data.</span></span>

## <a name="creating-the-storemanagercontroller"></a><span data-ttu-id="a96c4-112">Criando o StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="a96c4-112">Creating the StoreManagerController</span></span>

<span data-ttu-id="a96c4-113">Vamos começar criando um novo controlador chamado **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="a96c4-113">We'll begin by creating a new controller called **StoreManagerController**.</span></span> <span data-ttu-id="a96c4-114">Para este controlador, Extrairemos proveito dos recursos disponíveis na atualização das ferramentas do ASP.NET MVC 3 Scaffolding.</span><span class="sxs-lookup"><span data-stu-id="a96c4-114">For this controller, we will be taking advantage of the Scaffolding features available in the ASP.NET MVC 3 Tools Update.</span></span> <span data-ttu-id="a96c4-115">Defina as opções da caixa de diálogo Adicionar controlador, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="a96c4-115">Set the options for the Add Controller dialog as shown below.</span></span>

![](mvc-music-store-part-5/_static/image1.png)

<span data-ttu-id="a96c4-116">Quando você clica no botão Adicionar, você verá que o mecanismo de scaffolding do ASP.NET MVC 3 faz uma boa quantidade de trabalho para você:</span><span class="sxs-lookup"><span data-stu-id="a96c4-116">When you click the Add button, you'll see that the ASP.NET MVC 3 scaffolding mechanism does a good amount of work for you:</span></span>

- <span data-ttu-id="a96c4-117">Ele cria o novo StoreManagerController com uma variável local do Entity Framework</span><span class="sxs-lookup"><span data-stu-id="a96c4-117">It creates the new StoreManagerController with a local Entity Framework variable</span></span>
- <span data-ttu-id="a96c4-118">Ele adiciona uma pasta de StoreManager à pasta de modos de exibição do projeto</span><span class="sxs-lookup"><span data-stu-id="a96c4-118">It adds a StoreManager folder to the project's Views folder</span></span>
- <span data-ttu-id="a96c4-119">Ele adiciona a exibição Create. cshtml, DELETE. cshtml, details. cshtml, Edit. cshtml e index. cshtml, fortemente tipada para a classe do álbum</span><span class="sxs-lookup"><span data-stu-id="a96c4-119">It adds Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml, and Index.cshtml view, strongly typed to the Album class</span></span>

![](mvc-music-store-part-5/_static/image2.png)

<span data-ttu-id="a96c4-120">A nova classe de controlador StoreManager inclui CRUD (criar, ler, atualizar e excluir) ações do controlador que sabe como trabalhar com o álbum de classe de modelo e usar o nosso contexto de Entity Framework para acesso de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a96c4-120">The new StoreManager controller class includes CRUD (create, read, update, delete) controller actions which know how to work with the Album model class and use our Entity Framework context for database access.</span></span>

## <a name="modifying-a-scaffolded-view"></a><span data-ttu-id="a96c4-121">Modificação de uma exibição gerado por scaffolding</span><span class="sxs-lookup"><span data-stu-id="a96c4-121">Modifying a Scaffolded View</span></span>

<span data-ttu-id="a96c4-122">É importante lembrar que, embora esse código foi gerado para nós, ele é código ASP.NET MVC padrão, assim como escrevemos ao longo deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="a96c4-122">It's important to remember that, while this code was generated for us, it's standard ASP.NET MVC code, just like we've been writing throughout this tutorial.</span></span> <span data-ttu-id="a96c4-123">Ele tem se destina a economizar tempo que gastaria sobre como escrever o código do controlador clichê e criar as exibições fortemente tipadas manualmente, mas isso não é o tipo de código gerado que talvez você tenha visto precedido com avisos terríveis nos comentários sobre como você não deve alterar a código.</span><span class="sxs-lookup"><span data-stu-id="a96c4-123">It's intended to save you the time you'd spend on writing boilerplate controller code and creating the strongly typed views manually, but this isn't the kind of generated code you may have seen prefaced with dire warnings in comments about how you mustn't change the code.</span></span> <span data-ttu-id="a96c4-124">Este é seu código, e você deverá alterá-la.</span><span class="sxs-lookup"><span data-stu-id="a96c4-124">This is your code, and you're expected to change it.</span></span>

<span data-ttu-id="a96c4-125">Então, vamos começar com uma edição rápida para o modo de exibição de índice StoreManager (/ Views/StoreManager/Index.cshtml).</span><span class="sxs-lookup"><span data-stu-id="a96c4-125">So, let's start with a quick edit to the StoreManager Index view (/Views/StoreManager/Index.cshtml).</span></span> <span data-ttu-id="a96c4-126">Este modo de exibição exibe uma tabela que lista os álbuns em nosso repositório com editar / detalhes / excluir links e inclui as propriedades públicas do álbum.</span><span class="sxs-lookup"><span data-stu-id="a96c4-126">This view will display a table which lists the Albums in our store with Edit / Details / Delete links, and includes the Album's public properties.</span></span> <span data-ttu-id="a96c4-127">Vamos remover o campo AlbumArtUrl, pois ela não é muito útil nessa exibição.</span><span class="sxs-lookup"><span data-stu-id="a96c4-127">We'll remove the AlbumArtUrl field, as it's not very useful in this display.</span></span> <span data-ttu-id="a96c4-128">Na &lt;tabela&gt; seção do código de exibição, remover o &lt;th&gt; e &lt;td&gt; elementos ao redor AlbumArtUrl referências, conforme indicado pelas linhas destacadas abaixo:</span><span class="sxs-lookup"><span data-stu-id="a96c4-128">In &lt;table&gt; section of the view code, remove the &lt;th&gt; and &lt;td&gt; elements surrounding AlbumArtUrl references, as indicated by the highlighted lines below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

<span data-ttu-id="a96c4-129">O código de exibição modificado será exibida da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="a96c4-129">The modified view code will appear as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a><span data-ttu-id="a96c4-130">Um primeiro olhar o Store Manager</span><span class="sxs-lookup"><span data-stu-id="a96c4-130">A first look at the Store Manager</span></span>

<span data-ttu-id="a96c4-131">Agora execute o aplicativo e navegue até/StoreManager /.</span><span class="sxs-lookup"><span data-stu-id="a96c4-131">Now run the application and browse to /StoreManager/.</span></span> <span data-ttu-id="a96c4-132">Isso exibe apenas modificamos, mostrando uma lista dos álbuns no repositório com links para obter detalhes, editar e excluir o índice de Gerenciador de Store.</span><span class="sxs-lookup"><span data-stu-id="a96c4-132">This displays the Store Manager Index we just modified, showing a list of the albums in the store with links to Edit, Details, and Delete.</span></span>

![](mvc-music-store-part-5/_static/image3.png)

<span data-ttu-id="a96c4-133">Ao clicar no link de edição exibe um formulário de edição com campos para o álbum, incluindo menus suspensos para gênero e artista.</span><span class="sxs-lookup"><span data-stu-id="a96c4-133">Clicking the Edit link displays an edit form with fields for the Album, including dropdowns for Genre and Artist.</span></span>

![](mvc-music-store-part-5/_static/image4.png)

<span data-ttu-id="a96c4-134">Clique no link de "Volta à lista" na parte inferior, clique no link de detalhes para um álbum.</span><span class="sxs-lookup"><span data-stu-id="a96c4-134">Click the "Back to List" link at the bottom, then click on the Details link for an Album.</span></span> <span data-ttu-id="a96c4-135">Isso exibe informações detalhadas sobre um álbum individual.</span><span class="sxs-lookup"><span data-stu-id="a96c4-135">This displays the detail information for an individual Album.</span></span>

![](mvc-music-store-part-5/_static/image5.png)

<span data-ttu-id="a96c4-136">Novamente, clique o link voltar ao lista, clique em um link de Delete.</span><span class="sxs-lookup"><span data-stu-id="a96c4-136">Again, click the Back to List link, then click on a Delete link.</span></span> <span data-ttu-id="a96c4-137">Isso exibe uma caixa de diálogo de confirmação, mostrando os detalhes de álbum e perguntando se temos certeza de que deseja excluí-lo.</span><span class="sxs-lookup"><span data-stu-id="a96c4-137">This displays a confirmation dialog, showing the album details and asking if we're sure we want to delete it.</span></span>

![](mvc-music-store-part-5/_static/image6.png)

<span data-ttu-id="a96c4-138">Clicando no botão Excluir na parte inferior, exclui o álbum e de volta para a página de índice, que mostra o álbum excluído.</span><span class="sxs-lookup"><span data-stu-id="a96c4-138">Clicking the Delete button at the bottom will delete the album and return you to the Index page, which shows the album deleted.</span></span>

<span data-ttu-id="a96c4-139">Não terminamos com o Gerenciador de Store, mas temos controlador e o código de exibição para as operações CRUD iniciar a partir de trabalhar.</span><span class="sxs-lookup"><span data-stu-id="a96c4-139">We're not done with the Store Manager, but we have working controller and view code for the CRUD operations to start from.</span></span>

## <a name="looking-at-the-store-manager-controller-code"></a><span data-ttu-id="a96c4-140">Observando o código do controlador do Store Manager</span><span class="sxs-lookup"><span data-stu-id="a96c4-140">Looking at the Store Manager Controller code</span></span>

<span data-ttu-id="a96c4-141">O controlador do Store Manager contém uma boa quantidade de código.</span><span class="sxs-lookup"><span data-stu-id="a96c4-141">The Store Manager Controller contains a good amount of code.</span></span> <span data-ttu-id="a96c4-142">Vamos analisar isso de cima para baixo.</span><span class="sxs-lookup"><span data-stu-id="a96c4-142">Let's go through this from top to bottom.</span></span> <span data-ttu-id="a96c4-143">O controlador inclui alguns namespaces padrão para um controlador MVC, bem como uma referência ao namespace nossos modelos.</span><span class="sxs-lookup"><span data-stu-id="a96c4-143">The controller includes some standard namespaces for an MVC controller, as well as a reference to our Models namespace.</span></span> <span data-ttu-id="a96c4-144">O controlador tem uma instância particular da MusicStoreEntities, usado por cada uma das ações do controlador para acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="a96c4-144">The controller has a private instance of MusicStoreEntities, used by each of the controller actions for data access.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a><span data-ttu-id="a96c4-145">Store Manager Index e Details ações</span><span class="sxs-lookup"><span data-stu-id="a96c4-145">Store Manager Index and Details actions</span></span>

<span data-ttu-id="a96c4-146">O modo de exibição de índice recupera uma lista dos álbuns, inclusive referenciado gênero artista informações e do cada álbum, como vimos anteriormente ao trabalhar no método Store procurar.</span><span class="sxs-lookup"><span data-stu-id="a96c4-146">The index view retrieves a list of Albums, including each album's referenced Genre and Artist information, as we previously saw when working on the Store Browse method.</span></span> <span data-ttu-id="a96c4-147">O modo de exibição de índice é seguir as referências a objetos vinculados para que ele possa exibir cada álbum de nome de gênero e nome do artista, para que o controlador está sendo eficiente e de consulta para obter essas informações na solicitação original.</span><span class="sxs-lookup"><span data-stu-id="a96c4-147">The Index view is following the references to the linked objects so that it can display each album's Genre name and Artist name, so the controller is being efficient and querying for this information in the original request.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

<span data-ttu-id="a96c4-148">Ação do controlador de detalhes do controlador StoreManager funciona exatamente da mesma forma que a ação de detalhes do controlador de Store que escrevemos anteriormente - ele consulta para o álbum por ID usando o método Find (), em seguida, retorna para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="a96c4-148">The StoreManager Controller's Details controller action works exactly the same as the Store Controller Details action we wrote previously - it queries for the Album by ID using the Find() method, then returns it to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a><span data-ttu-id="a96c4-149">A criar métodos de ação</span><span class="sxs-lookup"><span data-stu-id="a96c4-149">The Create Action Methods</span></span>

<span data-ttu-id="a96c4-150">Os métodos de ação Criar são um pouco diferentes daquelas que vimos até agora, porque eles manipulam a entrada de formulário.</span><span class="sxs-lookup"><span data-stu-id="a96c4-150">The Create action methods are a little different from ones we've seen so far, because they handle form input.</span></span> <span data-ttu-id="a96c4-151">Quando um usuário visita primeiro /StoreManager/criar/eles serão mostrados um formulário vazio.</span><span class="sxs-lookup"><span data-stu-id="a96c4-151">When a user first visits /StoreManager/Create/ they will be shown an empty form.</span></span> <span data-ttu-id="a96c4-152">Essa página HTML conterá uma &lt;formulário&gt; onde eles podem digitar os detalhes do álbum de elementos de entrada do elemento que contém a lista suspensa e caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="a96c4-152">This HTML page will contain a &lt;form&gt; element that contains dropdown and textbox input elements where they can enter the album's details.</span></span>

<span data-ttu-id="a96c4-153">Depois que o usuário preenche os valores de formulário do álbum, pressione o botão "Salvar" para enviar que essas alterações de volta para o nosso aplicativo para salvar no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a96c4-153">After the user fills in the Album form values, they can press the "Save" button to submit these changes back to our application to save within the database.</span></span> <span data-ttu-id="a96c4-154">Quando o usuário pressiona o botão "Salvar" a &lt;formulário&gt; executará um HTTP-POST para a URL de /StoreManager/criar e enviar o &lt;formulário&gt; valores como parte de um HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="a96c4-154">When the user presses the "save" button the &lt;form&gt; will perform an HTTP-POST back to the /StoreManager/Create/ URL and submit the &lt;form&gt; values as part of the HTTP-POST.</span></span>

<span data-ttu-id="a96c4-155">ASP.NET MVC permite dividir facilmente a lógica desses dois cenários de invocação de URL, permitindo-nos implementar dois métodos de ação "Criar" separados dentro de nossa classe StoreManagerController – uma para tratar de HTTP GET inicial, navegue para o /StoreManager/Create / URL e o outro para lidar com HTTP-POST, as alterações enviadas.</span><span class="sxs-lookup"><span data-stu-id="a96c4-155">ASP.NET MVC allows us to easily split up the logic of these two URL invocation scenarios by enabling us to implement two separate "Create" action methods within our StoreManagerController class – one to handle the initial HTTP-GET browse to the /StoreManager/Create/ URL, and the other to handle the HTTP-POST of the submitted changes.</span></span>

### <a name="passing-information-to-a-view-using-viewbag"></a><span data-ttu-id="a96c4-156">Passar informações para uma exibição usando ViewBag</span><span class="sxs-lookup"><span data-stu-id="a96c4-156">Passing information to a View using ViewBag</span></span>

<span data-ttu-id="a96c4-157">Usei a ViewBag neste tutorial, mas ainda não falamos muito sobre ele.</span><span class="sxs-lookup"><span data-stu-id="a96c4-157">We've used the ViewBag earlier in this tutorial, but haven't talked much about it.</span></span> <span data-ttu-id="a96c4-158">A ViewBag nos permite passar informações para o modo de exibição sem usar um objeto de modelo fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="a96c4-158">The ViewBag allows us to pass information to the view without using a strongly typed model object.</span></span> <span data-ttu-id="a96c4-159">Nesse caso, nossa ação de controlador HTTP-GET Editar precisa passar uma lista de gêneros e artistas ao formulário para preencher os menus suspensos, e a maneira mais simples de fazer isso é para retorná-las como itens ViewBag.</span><span class="sxs-lookup"><span data-stu-id="a96c4-159">In this case, our Edit HTTP-GET controller action needs to pass both a list of Genres and Artists to the form to populate the dropdowns, and the simplest way to do that is to return them as ViewBag items.</span></span>

<span data-ttu-id="a96c4-160">A ViewBag é um objeto dinâmico, que significa que você pode digitar ViewBag.Foo ou ViewBag.YourNameHere sem escrever código para definir essas propriedades.</span><span class="sxs-lookup"><span data-stu-id="a96c4-160">The ViewBag is a dynamic object, meaning that you can type ViewBag.Foo or ViewBag.YourNameHere without writing code to define those properties.</span></span> <span data-ttu-id="a96c4-161">Nesse caso, o código do controlador usa ViewBag.GenreId e ViewBag.ArtistId para que os valores de lista suspensa enviados com o formulário serão GenreId e ArtistId, são eles definirá as propriedades do álbum.</span><span class="sxs-lookup"><span data-stu-id="a96c4-161">In this case, the controller code uses ViewBag.GenreId and ViewBag.ArtistId so that the dropdown values submitted with the form will be GenreId and ArtistId, which are the Album properties they will be setting.</span></span>

<span data-ttu-id="a96c4-162">Esses valores de lista suspensa são retornados para o formulário usando o objeto SelectList, que é criado apenas para essa finalidade.</span><span class="sxs-lookup"><span data-stu-id="a96c4-162">These dropdown values are returned to the form using the SelectList object, which is built just for that purpose.</span></span> <span data-ttu-id="a96c4-163">Isso é feito usando o código como este:</span><span class="sxs-lookup"><span data-stu-id="a96c4-163">This is done using code like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

<span data-ttu-id="a96c4-164">Como você pode ver no código do método de ação, três parâmetros estão sendo usados para criar esse objeto:</span><span class="sxs-lookup"><span data-stu-id="a96c4-164">As you can see from the action method code, three parameters are being used to create this object:</span></span>

- <span data-ttu-id="a96c4-165">A lista de itens que deseja exibir a lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="a96c4-165">The list of items the dropdown will be displaying.</span></span> <span data-ttu-id="a96c4-166">Observe que isso não é apenas uma cadeia de caracteres - estamos passando uma lista de gêneros.</span><span class="sxs-lookup"><span data-stu-id="a96c4-166">Note that this isn't just a string - we're passing a list of Genres.</span></span>
- <span data-ttu-id="a96c4-167">O próximo parâmetro que está sendo passado para o SelectList é o valor selecionado.</span><span class="sxs-lookup"><span data-stu-id="a96c4-167">The next parameter being passed to the SelectList is the Selected Value.</span></span> <span data-ttu-id="a96c4-168">Este como o SelectList sabe como pre-selecionar um item na lista.</span><span class="sxs-lookup"><span data-stu-id="a96c4-168">This how the SelectList knows how to pre-select an item in the list.</span></span> <span data-ttu-id="a96c4-169">Isso será mais fácil de entender quando olhamos o formulário de edição, que é muito semelhante.</span><span class="sxs-lookup"><span data-stu-id="a96c4-169">This will be easier to understand when we look at the Edit form, which is pretty similar.</span></span>
- <span data-ttu-id="a96c4-170">O parâmetro final é a propriedade a ser exibido.</span><span class="sxs-lookup"><span data-stu-id="a96c4-170">The final parameter is the property to be displayed.</span></span> <span data-ttu-id="a96c4-171">Nesse caso, isso está indicando que a propriedade Genre.Name é o que será mostrado ao usuário.</span><span class="sxs-lookup"><span data-stu-id="a96c4-171">In this case, this is indicating that the Genre.Name property is what will be shown to the user.</span></span>

<span data-ttu-id="a96c4-172">Com isso em mente, em seguida, a ação Criar HTTP GET é bastante simple – duas SelectLists são adicionados ao ViewBag e nenhum objeto de modelo é passado para o formulário (já que ele ainda não foi criado ainda).</span><span class="sxs-lookup"><span data-stu-id="a96c4-172">With that in mind, then, the HTTP-GET Create action is pretty simple - two SelectLists are added to the ViewBag, and no model object is passed to the form (since it hasn't been created yet).</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a><span data-ttu-id="a96c4-173">Auxiliares HTML para exibir a liste Drop no modo de exibição criar</span><span class="sxs-lookup"><span data-stu-id="a96c4-173">HTML Helpers to display the Drop Downs in the Create View</span></span>

<span data-ttu-id="a96c4-174">Uma vez que já falamos sobre como a lista suspensa de valores são passados para o modo de exibição, vamos dar uma olhada rápida na exibição para ver como esses valores são exibidos.</span><span class="sxs-lookup"><span data-stu-id="a96c4-174">Since we've talked about how the drop down values are passed to the view, let's take a quick look at the view to see how those values are displayed.</span></span> <span data-ttu-id="a96c4-175">No código do modo de exibição (/ Views/StoreManager/Create.cshtml), você verá a seguinte chamada é feita para exibir a lista de gênero para baixo.</span><span class="sxs-lookup"><span data-stu-id="a96c4-175">In the view code (/Views/StoreManager/Create.cshtml), you'll see the following call is made to display the Genre drop down.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

<span data-ttu-id="a96c4-176">Isso é conhecido como um auxiliar de HTML - um método de utilitário que executa uma tarefa comum de exibição.</span><span class="sxs-lookup"><span data-stu-id="a96c4-176">This is known as an HTML Helper - a utility method which performs a common view task.</span></span> <span data-ttu-id="a96c4-177">Os auxiliares HTML são muito úteis em manter nosso código de exibição concisas e legíveis.</span><span class="sxs-lookup"><span data-stu-id="a96c4-177">HTML Helpers are very useful in keeping our view code concise and readable.</span></span> <span data-ttu-id="a96c4-178">O auxiliar Html.DropDownList é fornecido pelo ASP.NET MVC, mas, como veremos posteriormente é possível criar nossos própria auxiliares para exibir o código que vai reutilizar em nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a96c4-178">The Html.DropDownList helper is provided by ASP.NET MVC, but as we'll see later it's possible to create our own helpers for view code we'll reuse in our application.</span></span>

<span data-ttu-id="a96c4-179">A chamada Html.DropDownList apenas precisa ser informado duas coisas - onde para obter a lista para exibir e qual valor (se houver) deve estar pré-selecionado.</span><span class="sxs-lookup"><span data-stu-id="a96c4-179">The Html.DropDownList call just needs to be told two things - where to get the list to display, and what value (if any) should be pre-selected.</span></span> <span data-ttu-id="a96c4-180">O primeiro parâmetro, GenreId, informa à DropDownList para procurar um valor chamado GenreId no modelo ou ViewBag.</span><span class="sxs-lookup"><span data-stu-id="a96c4-180">The first parameter, GenreId, tells the DropDownList to look for a value named GenreId in either the model or ViewBag.</span></span> <span data-ttu-id="a96c4-181">O segundo parâmetro é usado para indicar o valor para mostrar como inicialmente selecionado na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="a96c4-181">The second parameter is used to indicate the value to show as initially selected in the drop down list.</span></span> <span data-ttu-id="a96c4-182">Uma vez que esse formulário é uma forma de criar, não há nenhum valor a ser pré-selecionado e String. Empty é passado.</span><span class="sxs-lookup"><span data-stu-id="a96c4-182">Since this form is a Create form, there's no value to be preselected and String.Empty is passed.</span></span>

### <a name="handling-the-posted-form-values"></a><span data-ttu-id="a96c4-183">Lidar com os valores de formulário postados</span><span class="sxs-lookup"><span data-stu-id="a96c4-183">Handling the Posted Form values</span></span>

<span data-ttu-id="a96c4-184">Como abordado anteriormente, há dois métodos de ação associados a cada formulário.</span><span class="sxs-lookup"><span data-stu-id="a96c4-184">As we discussed before, there are two action methods associated with each form.</span></span> <span data-ttu-id="a96c4-185">O primeiro manipula a solicitação HTTP GET e exibe o formulário.</span><span class="sxs-lookup"><span data-stu-id="a96c4-185">The first handles the HTTP-GET request and displays the form.</span></span> <span data-ttu-id="a96c4-186">O segundo manipula a solicitação HTTP POST, que contém os valores de formulário enviado.</span><span class="sxs-lookup"><span data-stu-id="a96c4-186">The second handles the HTTP-POST request, which contains the submitted form values.</span></span> <span data-ttu-id="a96c4-187">Observe que a ação do controlador tem um atributo [HttpPost], que informa ao ASP.NET MVC que ele só deve responder a solicitações HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="a96c4-187">Notice that controller action has an [HttpPost] attribute, which tells ASP.NET MVC that it should only respond to HTTP-POST requests.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

<span data-ttu-id="a96c4-188">Essa ação tem quatro responsabilidades:</span><span class="sxs-lookup"><span data-stu-id="a96c4-188">This action has four responsibilities:</span></span>

- 1. <span data-ttu-id="a96c4-189">Ler os valores de formulário</span><span class="sxs-lookup"><span data-stu-id="a96c4-189">Read the form values</span></span>
- 2. <span data-ttu-id="a96c4-190">Verifique se os valores de formulário passam qualquer regra de validação</span><span class="sxs-lookup"><span data-stu-id="a96c4-190">Check if the form values pass any validation rules</span></span>
- 3. <span data-ttu-id="a96c4-191">Se o envio do formulário for válido, salve os dados e exibir a lista atualizada</span><span class="sxs-lookup"><span data-stu-id="a96c4-191">If the form submission is valid, save the data and display the updated list</span></span>
- 4. <span data-ttu-id="a96c4-192">Se o envio do formulário não é válido, reexiba o formulário com erros de validação</span><span class="sxs-lookup"><span data-stu-id="a96c4-192">If the form submission is not valid, redisplay the form with validation errors</span></span>

#### <a name="reading-form-values-with-model-binding"></a><span data-ttu-id="a96c4-193">Ler valores de formulário com associação de modelos</span><span class="sxs-lookup"><span data-stu-id="a96c4-193">Reading Form Values with Model Binding</span></span>

<span data-ttu-id="a96c4-194">A ação do controlador está processando o envio de um formulário que inclui valores para GenreId e ArtistId (na lista suspensa) e os valores de caixa de texto de título, preço e AlbumArtUrl.</span><span class="sxs-lookup"><span data-stu-id="a96c4-194">The controller action is processing a form submission that includes values for GenreId and ArtistId (from the drop down list) and textbox values for Title, Price, and AlbumArtUrl.</span></span> <span data-ttu-id="a96c4-195">Embora seja possível acessar diretamente os valores de formulário, uma abordagem melhor é usar os recursos de associação de modelos integrados ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a96c4-195">While it's possible to directly access form values, a better approach is to use the Model Binding capabilities built into ASP.NET MVC.</span></span> <span data-ttu-id="a96c4-196">Quando uma ação do controlador usa um tipo de modelo como um parâmetro, o ASP.NET MVC tentará popular um objeto desse tipo usando entradas de formulário (bem como valores de rota e de cadeia de consulta).</span><span class="sxs-lookup"><span data-stu-id="a96c4-196">When a controller action takes a model type as a parameter, ASP.NET MVC will attempt to populate an object of that type using form inputs (as well as route and querystring values).</span></span> <span data-ttu-id="a96c4-197">Isso é feito procurando valores cujos nomes correspondem a propriedades do objeto de modelo, por exemplo, ao definir o novo álbum valor do objeto GenreId, ele procura uma entrada com o nome GenreId.</span><span class="sxs-lookup"><span data-stu-id="a96c4-197">It does this by looking for values whose names match properties of the model object, e.g. when setting the new Album object's GenreId value, it looks for an input with the name GenreId.</span></span> <span data-ttu-id="a96c4-198">Ao criar modos de exibição usando os métodos padrão no ASP.NET MVC, os formulários sempre serão processados usando nomes de propriedades como nomes de campo de entrada, portanto, isso os nomes de campo serão apenas corresponder ao.</span><span class="sxs-lookup"><span data-stu-id="a96c4-198">When you create views using the standard methods in ASP.NET MVC, the forms will always be rendered using property names as input field names, so this the field names will just match up.</span></span>

#### <a name="validating-the-model"></a><span data-ttu-id="a96c4-199">Validação do modelo</span><span class="sxs-lookup"><span data-stu-id="a96c4-199">Validating the Model</span></span>

<span data-ttu-id="a96c4-200">O modelo é validado com uma chamada simples para o ModelState.</span><span class="sxs-lookup"><span data-stu-id="a96c4-200">The model is validated with a simple call to ModelState.IsValid.</span></span> <span data-ttu-id="a96c4-201">Ainda não adicionamos qualquer regra de validação à nossa classe de álbum ainda – vamos fazer isso daqui a pouco – agora essa verificação não têm muita relação.</span><span class="sxs-lookup"><span data-stu-id="a96c4-201">We haven't added any validation rules to our Album class yet - we'll do that in a bit - so right now this check doesn't have much to do.</span></span> <span data-ttu-id="a96c4-202">O importante é que essa verificação ModelStat.IsValid adaptará às regras de validação, colocamos em nosso modelo, para que as alterações futuras em regras de validação não exigirá nenhuma atualização para o código de ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="a96c4-202">What's important is that this ModelStat.IsValid check will adapt to the validation rules we put on our model, so future changes to validation rules won't require any updates to the controller action code.</span></span>

#### <a name="saving-the-submitted-values"></a><span data-ttu-id="a96c4-203">Salvando os valores enviados</span><span class="sxs-lookup"><span data-stu-id="a96c4-203">Saving the submitted values</span></span>

<span data-ttu-id="a96c4-204">Se o envio do formulário passar na validação, é hora de salvar os valores no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a96c4-204">If the form submission passes validation, it's time to save the values to the database.</span></span> <span data-ttu-id="a96c4-205">Com o Entity Framework, que requer apenas adicionar modelo à coleção de álbuns e chamar SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="a96c4-205">With Entity Framework, that just requires adding the model to the Albums collection and calling SaveChanges.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

<span data-ttu-id="a96c4-206">Entity Framework gera os comandos SQL apropriados para manter o valor.</span><span class="sxs-lookup"><span data-stu-id="a96c4-206">Entity Framework generates the appropriate SQL commands to persist the value.</span></span> <span data-ttu-id="a96c4-207">Se o texto fornecido é menor que o comprimento especificado, o auxiliar de saídas-lo como-está.</span><span class="sxs-lookup"><span data-stu-id="a96c4-207">After saving the data, we redirect back to the list of Albums so we can see our update.</span></span> <span data-ttu-id="a96c4-208">Se for mais longo, ele trunca o texto e renderiza..."para o restante.</span><span class="sxs-lookup"><span data-stu-id="a96c4-208">This is done by returning RedirectToAction with the name of the controller action we want displayed.</span></span> <span data-ttu-id="a96c4-209">Agora podemos usar nosso auxiliar de truncamento para garantir que as propriedades de título do álbum tanto o nome do artista são menos de 25 caracteres.</span><span class="sxs-lookup"><span data-stu-id="a96c4-209">In this case, that's the Index method.</span></span>

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a><span data-ttu-id="a96c4-210">O código de exibição completa usando nosso novo auxiliar de truncamento é exibida abaixo.</span><span class="sxs-lookup"><span data-stu-id="a96c4-210">Displaying invalid form submissions with Validation Errors</span></span>

<span data-ttu-id="a96c4-211">Agora quando pesquisamos a URL /StoreManager/, os álbuns e títulos são mantidos abaixo nosso comprimentos máximos.</span><span class="sxs-lookup"><span data-stu-id="a96c4-211">In the case of invalid form input, the dropdown values are added to the ViewBag (as in the HTTP-GET case) and the bound model values are passed back to the view for display.</span></span> <span data-ttu-id="a96c4-212">Observação: Isso mostra o caso mais simples de criar e usar um auxiliar em um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="a96c4-212">Validation errors are automatically displayed using the @Html.ValidationMessageFor HTML Helper.</span></span>

#### <a name="testing-the-create-form"></a><span data-ttu-id="a96c4-213">Para saber mais sobre a criação de auxiliares que podem ser usados em todo o site, consulte a postagem do meu blog:</span><span class="sxs-lookup"><span data-stu-id="a96c4-213">Testing the Create Form</span></span>

<span data-ttu-id="a96c4-214">Para testar isso, execute o aplicativo e navegue até /StoreManager/criar / - Isso mostrará o formulário em branco que foi retornado pelo método StoreController criar HTTP-GET.</span><span class="sxs-lookup"><span data-stu-id="a96c4-214">To test this out, run the application and browse to /StoreManager/Create/ - this will show you the blank form which was returned by the StoreController Create HTTP-GET method.</span></span>

<span data-ttu-id="a96c4-215">Preencha alguns valores e clique no botão Criar para enviar o formulário.</span><span class="sxs-lookup"><span data-stu-id="a96c4-215">Fill in some values and click the Create button to submit the form.</span></span>

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a><span data-ttu-id="a96c4-216">Tratamento de edições</span><span class="sxs-lookup"><span data-stu-id="a96c4-216">Handling Edits</span></span>

<span data-ttu-id="a96c4-217">O par de ação Editar (HTTP-GET e HTTP POST) é muito semelhante aos métodos de ação Criar que acabamos de ver.</span><span class="sxs-lookup"><span data-stu-id="a96c4-217">The Edit action pair (HTTP-GET and HTTP-POST) are very similar to the Create action methods we just looked at.</span></span> <span data-ttu-id="a96c4-218">Uma vez que o cenário de edição envolve o trabalho com um álbum existente, editar HTTP-GET método carrega o álbum de acordo com o parâmetro de "id", passado por meio da rota.</span><span class="sxs-lookup"><span data-stu-id="a96c4-218">Since the edit scenario involves working with an existing album, the Edit HTTP-GET method loads the Album based on the "id" parameter, passed in via the route.</span></span> <span data-ttu-id="a96c4-219">Esse código para recuperar um álbum por AlbumId é o mesmo anteriormente examinamos na ação de controlador de detalhes.</span><span class="sxs-lookup"><span data-stu-id="a96c4-219">This code for retrieving an album by AlbumId is the same as we've previously looked at in the Details controller action.</span></span> <span data-ttu-id="a96c4-220">Assim como acontece com a criação / método HTTP GET, na lista suspensa de valores são retornados por meio de ViewBag.</span><span class="sxs-lookup"><span data-stu-id="a96c4-220">As with the Create / HTTP-GET method, the drop down values are returned via the ViewBag.</span></span> <span data-ttu-id="a96c4-221">Isso nos permite retornar um álbum como nosso objeto de modelo para o modo de exibição (que é fortemente tipado para a classe de álbum) ao transmitir dados adicionais (por exemplo, uma lista de gêneros) por meio de ViewBag.</span><span class="sxs-lookup"><span data-stu-id="a96c4-221">This allows us to return an Album as our model object to the view (which is strongly typed to the Album class) while passing additional data (e.g. a list of Genres) via the ViewBag.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

<span data-ttu-id="a96c4-222">A ação HTTP-POST editar é muito semelhante à ação HTTP-POST criar.</span><span class="sxs-lookup"><span data-stu-id="a96c4-222">The Edit HTTP-POST action is very similar to the Create HTTP-POST action.</span></span> <span data-ttu-id="a96c4-223">A única diferença é que, em vez de adicionar um novo álbum ao banco de dados. Coleção de álbuns, podemos está localizando a instância atual do álbum usando o banco de dados. Entry(Album) e definindo seu estado para modificado.</span><span class="sxs-lookup"><span data-stu-id="a96c4-223">The only difference is that instead of adding a new album to the db.Albums collection, we're finding the current instance of the Album using db.Entry(album) and setting its state to Modified.</span></span> <span data-ttu-id="a96c4-224">Isso informa ao Entity Framework que estamos modificando um álbum existente em vez de criar um novo.</span><span class="sxs-lookup"><span data-stu-id="a96c4-224">This tells Entity Framework that we are modifying an existing album as opposed to creating a new one.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

<span data-ttu-id="a96c4-225">Podemos testar isso executando o aplicativo e navegando até/StoreManger /, em seguida, clicar no link de edição para um álbum.</span><span class="sxs-lookup"><span data-stu-id="a96c4-225">We can test this out by running the application and browsing to /StoreManger/, then clicking the Edit link for an album.</span></span>

![](mvc-music-store-part-5/_static/image9.png)

<span data-ttu-id="a96c4-226">Isso exibe o formulário de edição, mostrado pelo método HTTP-GET editar.</span><span class="sxs-lookup"><span data-stu-id="a96c4-226">This displays the Edit form shown by the Edit HTTP-GET method.</span></span> <span data-ttu-id="a96c4-227">Preencha alguns valores e clique no botão Salvar.</span><span class="sxs-lookup"><span data-stu-id="a96c4-227">Fill in some values and click the Save button.</span></span>

![](mvc-music-store-part-5/_static/image10.png)

<span data-ttu-id="a96c4-228">Isso envia o formulário, salva os valores e retorna-à lista de álbum, mostrando que os valores foram atualizados.</span><span class="sxs-lookup"><span data-stu-id="a96c4-228">This posts the form, saves the values, and returns us to the Album list, showing that the values were updated.</span></span>

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a><span data-ttu-id="a96c4-229">Tratamento de exclusão</span><span class="sxs-lookup"><span data-stu-id="a96c4-229">Handling Deletion</span></span>

<span data-ttu-id="a96c4-230">Exclusão segue o mesmo padrão que editar e criar, usando a ação de um controlador para exibir o formulário de confirmação e outra ação do controlador para manipular o envio do formulário.</span><span class="sxs-lookup"><span data-stu-id="a96c4-230">Deletion follows the same pattern as Edit and Create, using one controller action to display the confirmation form, and another controller action to handle the form submission.</span></span>

<span data-ttu-id="a96c4-231">A ação do controlador GET HTTP Delete é exatamente o mesmo que nossa ação de controlador Store Manager detalhes anterior.</span><span class="sxs-lookup"><span data-stu-id="a96c4-231">The HTTP-GET Delete controller action is exactly the same as our previous Store Manager Details controller action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

<span data-ttu-id="a96c4-232">Podemos exibir um formulário que é fortemente tipado a um tipo de álbum, usando o modelo de conteúdo de modo de exibição de exclusão.</span><span class="sxs-lookup"><span data-stu-id="a96c4-232">We display a form that's strongly typed to an Album type, using the Delete view content template.</span></span>

![](mvc-music-store-part-5/_static/image12.png)

<span data-ttu-id="a96c4-233">O modelo de exclusão mostra todos os campos para o modelo, mas podemos simplificar bastante essa busca.</span><span class="sxs-lookup"><span data-stu-id="a96c4-233">The Delete template shows all the fields for the model, but we can simplify that down quite a bit.</span></span> <span data-ttu-id="a96c4-234">Altere o código de exibição no /Views/StoreManager/Delete.cshtml ao seguinte.</span><span class="sxs-lookup"><span data-stu-id="a96c4-234">Change the view code in /Views/StoreManager/Delete.cshtml to the following.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

<span data-ttu-id="a96c4-235">Isso exibe uma confirmação de exclusão simplificada.</span><span class="sxs-lookup"><span data-stu-id="a96c4-235">This displays a simplified Delete confirmation.</span></span>

![](mvc-music-store-part-5/_static/image13.png)

<span data-ttu-id="a96c4-236">Clicando no botão Excluir faz com que o formulário a ser postada de volta para o servidor que executa a ação DeleteConfirmed.</span><span class="sxs-lookup"><span data-stu-id="a96c4-236">Clicking the Delete button causes the form to be posted back to the server, which executes the DeleteConfirmed action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

<span data-ttu-id="a96c4-237">Nossa ação de controlador HTTP-POST excluir realiza as seguintes ações:</span><span class="sxs-lookup"><span data-stu-id="a96c4-237">Our HTTP-POST Delete Controller Action takes the following actions:</span></span>

- 1. <span data-ttu-id="a96c4-238">Carrega o álbum por ID</span><span class="sxs-lookup"><span data-stu-id="a96c4-238">Loads the Album by ID</span></span>
- 2. <span data-ttu-id="a96c4-239">Exclui o álbum e salvar as alterações</span><span class="sxs-lookup"><span data-stu-id="a96c4-239">Deletes it the album and save changes</span></span>
- 3. <span data-ttu-id="a96c4-240">Redireciona para o índice, mostrando que o álbum foi removido da lista</span><span class="sxs-lookup"><span data-stu-id="a96c4-240">Redirects to the Index, showing that the Album was removed from the list</span></span>

<span data-ttu-id="a96c4-241">Para testar isso, execute o aplicativo e navegue até /StoreManager.</span><span class="sxs-lookup"><span data-stu-id="a96c4-241">To test this, run the application and browse to /StoreManager.</span></span> <span data-ttu-id="a96c4-242">Selecione um álbum na lista e clique no link excluir.</span><span class="sxs-lookup"><span data-stu-id="a96c4-242">Select an album from the list and click the Delete link.</span></span>

![](mvc-music-store-part-5/_static/image14.png)

<span data-ttu-id="a96c4-243">Isso exibe nossa tela de confirmação de exclusão.</span><span class="sxs-lookup"><span data-stu-id="a96c4-243">This displays our Delete confirmation screen.</span></span>

![](mvc-music-store-part-5/_static/image15.png)

<span data-ttu-id="a96c4-244">Clicando no botão Excluir remove o álbum e retorna-nos para a página de índice do Store Manager, que mostra que o álbum foi excluído.</span><span class="sxs-lookup"><span data-stu-id="a96c4-244">Clicking the Delete button removes the album and returns us to the Store Manager Index page, which shows that the album has been deleted.</span></span>

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a><span data-ttu-id="a96c4-245">Usando um auxiliar de HTML personalizados para truncar o texto</span><span class="sxs-lookup"><span data-stu-id="a96c4-245">Using a custom HTML Helper to truncate text</span></span>

<span data-ttu-id="a96c4-246">Temos um problema potencial com nossa página de índice do Store Manager.</span><span class="sxs-lookup"><span data-stu-id="a96c4-246">We've got one potential issue with our Store Manager Index page.</span></span> <span data-ttu-id="a96c4-247">Nossas propriedades do título do álbum e o nome do artista podem ambos ser longo o suficiente que eles poderia lançar desativar a formatação de nossa tabela.</span><span class="sxs-lookup"><span data-stu-id="a96c4-247">Our Album Title and Artist Name properties can both be long enough that they could throw off our table formatting.</span></span> <span data-ttu-id="a96c4-248">Vamos criar um auxiliar de HTML personalizados para permitir que nós truncar facilmente essas e outras propriedades em nossos modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="a96c4-248">We'll create a custom HTML Helper to allow us to easily truncate these and other properties in our Views.</span></span>

![](mvc-music-store-part-5/_static/image17.png)

<span data-ttu-id="a96c4-249">Do Razor @helper sintaxe tornou muito fácil criar suas próprias funções auxiliares para uso em exibições.</span><span class="sxs-lookup"><span data-stu-id="a96c4-249">Razor's @helper syntax has made it pretty easy to create your own helper functions for use in your views.</span></span> <span data-ttu-id="a96c4-250">Abra o modo de exibição /Views/StoreManager/Index.cshtml e adicione o código a seguir diretamente após o @model linha.</span><span class="sxs-lookup"><span data-stu-id="a96c4-250">Open the /Views/StoreManager/Index.cshtml view and add the following code directly after the @model line.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

<span data-ttu-id="a96c4-251">Esse método auxiliar usa uma cadeia de caracteres e um comprimento máximo para permitir.</span><span class="sxs-lookup"><span data-stu-id="a96c4-251">This helper method takes a string and a maximum length to allow.</span></span> <span data-ttu-id="a96c4-252">Se o texto fornecido é menor que o comprimento especificado, o auxiliar de saídas-lo como-está.</span><span class="sxs-lookup"><span data-stu-id="a96c4-252">If the text supplied is shorter than the length specified, the helper outputs it as-is.</span></span> <span data-ttu-id="a96c4-253">Se for mais longo, ele trunca o texto e renderiza..."para o restante.</span><span class="sxs-lookup"><span data-stu-id="a96c4-253">If it is longer, then it truncates the text and renders "…" for the remainder.</span></span>

<span data-ttu-id="a96c4-254">Agora podemos usar nosso auxiliar de truncamento para garantir que as propriedades de título do álbum tanto o nome do artista são menos de 25 caracteres.</span><span class="sxs-lookup"><span data-stu-id="a96c4-254">Now we can use our Truncate helper to ensure that both the Album Title and Artist Name properties are less than 25 characters.</span></span> <span data-ttu-id="a96c4-255">O código de exibição completa usando nosso novo auxiliar de truncamento é exibida abaixo.</span><span class="sxs-lookup"><span data-stu-id="a96c4-255">The complete view code using our new Truncate helper appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

<span data-ttu-id="a96c4-256">Agora quando pesquisamos a URL /StoreManager/, os álbuns e títulos são mantidos abaixo nosso comprimentos máximos.</span><span class="sxs-lookup"><span data-stu-id="a96c4-256">Now when we browse the /StoreManager/ URL, the albums and titles are kept below our maximum lengths.</span></span>

![](mvc-music-store-part-5/_static/image18.png)

<span data-ttu-id="a96c4-257">Observação: Isso mostra o caso mais simples de criar e usar um auxiliar em um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="a96c4-257">Note: This shows the simple case of creating and using a helper in one view.</span></span> <span data-ttu-id="a96c4-258">Para saber mais sobre a criação de auxiliares que podem ser usados em todo o site, consulte a postagem do meu blog: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span><span class="sxs-lookup"><span data-stu-id="a96c4-258">To learn more about creating helpers that you can use throughout your site, see my blog post: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="a96c4-259">[Anterior](mvc-music-store-part-4.md)
> [Próximo](mvc-music-store-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="a96c4-259">[Previous](mvc-music-store-part-4.md)
[Next](mvc-music-store-part-6.md)</span></span>

---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Parte 3: Exibições e ViewModels | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 3 aborda Views e ViewModels.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 8fd89c2a448877bf13a7828f545ffcd400f63bb1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837404"
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="3b265-104">Parte 3: Exibições e ViewModels</span><span class="sxs-lookup"><span data-stu-id="3b265-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="3b265-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="3b265-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="3b265-106">A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.</span><span class="sxs-lookup"><span data-stu-id="3b265-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="3b265-107">A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="3b265-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="3b265-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3b265-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="3b265-109">Parte 3 aborda Views e ViewModels.</span><span class="sxs-lookup"><span data-stu-id="3b265-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="3b265-110">Até agora podemos ter apenas foi retornar cadeias de caracteres de ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="3b265-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="3b265-111">Isso é uma excelente forma de obter uma ideia de como funcionam os controladores, mas é não como convém criar um aplicativo web real.</span><span class="sxs-lookup"><span data-stu-id="3b265-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="3b265-112">Vamos querer uma maneira melhor de gerar HTML para navegadores visitar nosso site – uma em que podemos usar arquivos de modelo para personalizar mais facilmente o conteúdo HTML enviada de volta.</span><span class="sxs-lookup"><span data-stu-id="3b265-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="3b265-113">Isso é exatamente o que fazem os modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="3b265-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="3b265-114">Adicionando um modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="3b265-114">Adding a View template</span></span>

<span data-ttu-id="3b265-115">Para usar um modelo de exibição, vamos alterar o método Index do HomeController para retornar um ActionResult e que ele retorne View(), como abaixo:</span><span class="sxs-lookup"><span data-stu-id="3b265-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="3b265-116">A alteração acima indica que, em vez de retornou uma cadeia de caracteres, em vez disso, queremos usar uma "exibição" para gerar um resultado de volta.</span><span class="sxs-lookup"><span data-stu-id="3b265-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="3b265-117">Agora vamos adicionar um modelo de exibição apropriado ao nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="3b265-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="3b265-118">Para fazer isso, podemos posicionar o cursor do texto dentro do método de ação de índice, e em seguida, clique com botão direito e selecione "Adicionar exibição".</span><span class="sxs-lookup"><span data-stu-id="3b265-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="3b265-119">Isso abrirá a caixa de diálogo Adicionar modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="3b265-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="3b265-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3b265-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="3b265-121">A caixa de diálogo "Adicionar exibição" nos permite rapidamente e facilmente gerar arquivos de modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="3b265-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="3b265-122">Por padrão "Adicionar exibição de" caixa de diálogo preenche previamente o nome do modelo de exibição para criar para que ele corresponda ao método de ação que irá usá-lo.</span><span class="sxs-lookup"><span data-stu-id="3b265-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="3b265-123">Como usamos o menu de contexto "Adicionar exibição" dentro do método de ação Index () de nossa HomeController, a caixa de diálogo "Adicionar exibição" acima tem "Index" como o nome de exibição previamente preenchido por padrão.</span><span class="sxs-lookup"><span data-stu-id="3b265-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="3b265-124">Não precisamos alterar qualquer uma das opções na caixa de diálogo, então, clique no botão Adicionar.</span><span class="sxs-lookup"><span data-stu-id="3b265-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="3b265-125">Quando clicamos no botão Add, o Visual Web Developer criará um index. cshtml novo modelo de exibição para nós no diretório \Views\Home, criando a pasta se ainda não existe.</span><span class="sxs-lookup"><span data-stu-id="3b265-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="3b265-126">O nome e uma pasta local do arquivo "Index. cshtml" é importante e segue as convenções de nomenclatura padrão ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3b265-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="3b265-127">O nome do diretório, \Views\Home, corresponde ao controlador - que é o nome HomeController.</span><span class="sxs-lookup"><span data-stu-id="3b265-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="3b265-128">O nome de modelo de exibição, índice, coincide com o método de ação do controlador que deseja exibir o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="3b265-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="3b265-129">ASP.NET MVC permite evitar ter que especificar explicitamente o nome ou local de um modelo de exibição quando usamos essa convenção de nomenclatura para retornar uma exibição.</span><span class="sxs-lookup"><span data-stu-id="3b265-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="3b265-130">Por padrão renderizará o modelo de exibição \Views\Home\Index.cshtml quando escrevemos código, como a seguir dentro do nosso HomeController:</span><span class="sxs-lookup"><span data-stu-id="3b265-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="3b265-131">O Visual Web Developer criado e aberto o modelo de exibição "Index. cshtml", depois, clicamos no botão "Adicionar" na caixa de diálogo "Adicionar exibição".</span><span class="sxs-lookup"><span data-stu-id="3b265-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="3b265-132">O conteúdo de index. cshtml é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="3b265-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="3b265-133">Essa exibição está usando a sintaxe do Razor, que é mais concisa que o mecanismo de exibição de formulários da Web usado em Web Forms do ASP.NET e as versões anteriores do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3b265-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="3b265-134">O mecanismo de exibição de Web Forms ainda está disponível no ASP.NET MVC 3, mas muitos desenvolvedores acham que o mecanismo de exibição do Razor se encaixa o desenvolvimento de ASP.NET MVC muito bem.</span><span class="sxs-lookup"><span data-stu-id="3b265-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="3b265-135">As três primeiras linhas definem o título da página usando ViewBag.</span><span class="sxs-lookup"><span data-stu-id="3b265-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="3b265-136">Vamos examinar como isso funciona em mais detalhes em breve, mas primeiro vamos atualizar o texto de cabeçalho do texto e exibir a página.</span><span class="sxs-lookup"><span data-stu-id="3b265-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="3b265-137">Atualizar o &lt;h2&gt; marca dizer "Esta é a Home Page", conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="3b265-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="3b265-138">Executando a aplicativo mostra que nosso novo texto está visível na home page.</span><span class="sxs-lookup"><span data-stu-id="3b265-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="3b265-139">Usando um Layout para elementos comuns do site</span><span class="sxs-lookup"><span data-stu-id="3b265-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="3b265-140">A maioria dos sites têm conteúdo que é compartilhado entre várias páginas: navegação, rodapés de páginas, imagens de logotipo, referências de folha de estilos, etc. O mecanismo de exibição Razor torna isso fácil de gerenciar o uso de uma página chamada \_layout. cshtml, que automaticamente foi criado para nós dentro da pasta/Views/Shared.</span><span class="sxs-lookup"><span data-stu-id="3b265-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="3b265-141">Clique duas vezes nessa pasta para exibir o conteúdo, que são mostrados abaixo.</span><span class="sxs-lookup"><span data-stu-id="3b265-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="3b265-142">O conteúdo do nosso exibições individuais será exibido pelo @RenderBodycomando () e qualquer conteúdo comum que desejamos aparecer fora que podem ser adicionados para o \_marcação de layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="3b265-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="3b265-143">Vamos nosso Store de música do MVC para ter um cabeçalho comum com links para nossa área Home page e Store em todas as páginas no site, portanto, vamos adicionar isso ao modelo diretamente acima que @RenderBodyinstrução ().</span><span class="sxs-lookup"><span data-stu-id="3b265-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="3b265-144">Atualizando a folha de estilos</span><span class="sxs-lookup"><span data-stu-id="3b265-144">Updating the StyleSheet</span></span>

<span data-ttu-id="3b265-145">O modelo de projeto vazio inclui um arquivo CSS muito simplificado que inclui apenas os estilos usados para exibir mensagens de validação.</span><span class="sxs-lookup"><span data-stu-id="3b265-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="3b265-146">Nosso designer forneceu alguns CSS e imagens adicionais para definir a aparência do nosso site, portanto, vamos adicionar os agora.</span><span class="sxs-lookup"><span data-stu-id="3b265-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="3b265-147">O arquivo CSS e imagens atualizados estão incluídas no diretório de conteúdo de Assets.zip MvcMusicStore que está disponível em [MVC de música de Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="3b265-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="3b265-148">Vamos selecionar ambos no Windows Explorer e soltá-los na pasta de conteúdo da nossa solução no Visual Web Developer, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="3b265-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="3b265-149">Você será solicitado a confirmar se deseja substituir o arquivo CSS existente.</span><span class="sxs-lookup"><span data-stu-id="3b265-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="3b265-150">Clique em Sim.</span><span class="sxs-lookup"><span data-stu-id="3b265-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="3b265-151">A pasta de conteúdo do seu aplicativo agora será exibida da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3b265-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="3b265-152">Agora vamos executar o aplicativo e ver a aparecem de nossas alterações na Home page.</span><span class="sxs-lookup"><span data-stu-id="3b265-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="3b265-153">Vamos examinar o que mudou: método de ação de índice o HomeController encontrado e exibido modelo \Views\Home\Index.cshtmlView, mesmo que o nosso código chamado "retorno View()", pois nosso modelo de exibição seguido a convenção de nomenclatura padrão.</span><span class="sxs-lookup"><span data-stu-id="3b265-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="3b265-154">A Home Page é exibindo uma mensagem de boas-vinda simples que é definida dentro do modelo de exibição \Views\Home\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="3b265-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="3b265-155">A Home Page é usando nossa \_modelo de layout. cshtml, e, portanto, a mensagem de boas-vinda está contida o layout do site padrão HTML.</span><span class="sxs-lookup"><span data-stu-id="3b265-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="3b265-156">Usando um modelo para passar informações para nossa exibição</span><span class="sxs-lookup"><span data-stu-id="3b265-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="3b265-157">Um modelo de exibição que exibe apenas inserido no código HTML não vai deixar um site muito interessante.</span><span class="sxs-lookup"><span data-stu-id="3b265-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="3b265-158">Para criar um site dinâmico, em vez disso, vamos passar informações de nossas ações do controller para nossos modelos de exibição.</span><span class="sxs-lookup"><span data-stu-id="3b265-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="3b265-159">O padrão Model-View-Controller, o termo A que modelo se refere objetos que representam os dados no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3b265-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="3b265-160">Muitas vezes, objetos de modelo correspondem às tabelas no banco de dados, mas eles não precisem.</span><span class="sxs-lookup"><span data-stu-id="3b265-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="3b265-161">Os métodos de ação do controlador que retornar um ActionResult podem passar um objeto de modelo para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="3b265-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="3b265-162">Isso permite que um controlador empacotar corretamente todas as informações necessárias para gerar uma resposta e, em seguida, passar essas informações as a um modelo de exibição para usar para gerar a resposta apropriada do HTML.</span><span class="sxs-lookup"><span data-stu-id="3b265-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="3b265-163">Isso é mais fácil de entender, conferindo-lo em ação, então vamos começar.</span><span class="sxs-lookup"><span data-stu-id="3b265-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="3b265-164">Primeiro, criaremos algumas classes de modelo para representar gêneros e álbuns em nosso armazenamento.</span><span class="sxs-lookup"><span data-stu-id="3b265-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="3b265-165">Vamos começar criando uma classe de gênero.</span><span class="sxs-lookup"><span data-stu-id="3b265-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="3b265-166">Clique com botão direito na pasta "Modelos" dentro de seu projeto, escolha a opção de ""adicionar Class e nomeie o arquivo "Genre.cs".</span><span class="sxs-lookup"><span data-stu-id="3b265-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="3b265-167">Em seguida, adicione uma propriedade de nome da cadeia de caracteres pública à classe que foi criada:</span><span class="sxs-lookup"><span data-stu-id="3b265-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="3b265-168">*Observação: no caso você esteja se perguntando, o {get; definir;} notação está fazendo uso do recurso de Propriedades autoimplementadas do #. Isso nos dá os benefícios de uma propriedade sem a necessidade de nós declarar um campo de suporte.*</span><span class="sxs-lookup"><span data-stu-id="3b265-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="3b265-169">Em seguida, siga as mesmas etapas para criar uma classe de álbum (chamada Album.cs) que tem um título e uma propriedade de Gênero:</span><span class="sxs-lookup"><span data-stu-id="3b265-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="3b265-170">Agora podemos modificar o StoreController para usar os modos de exibição que exibem informações dinâmicas do nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="3b265-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="3b265-171">Se - para fins de demonstração no momento - nomeamos o nosso álbuns com base na ID de solicitação, podemos pode exibir essas informações como no modo de exibição abaixo.</span><span class="sxs-lookup"><span data-stu-id="3b265-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="3b265-172">Vamos começar alterando a ação de detalhes da Store para que ela mostre as informações para um único álbum.</span><span class="sxs-lookup"><span data-stu-id="3b265-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="3b265-173">Adicione uma instrução "using" na parte superior do **StoreControllers** classe para incluir o namespace MvcMusicStore.Models, portanto, não precisamos digitar MvcMusicStore.Models.Album sempre que queremos usar a classe de álbum.</span><span class="sxs-lookup"><span data-stu-id="3b265-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="3b265-174">A seção "usos" dessa classe agora deve aparecer conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="3b265-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="3b265-175">Em seguida, vamos atualizar a ação do controlador de detalhes para que ele retorne um ActionResult em vez de uma cadeia de caracteres, como fizemos com o método de índice do HomeController.</span><span class="sxs-lookup"><span data-stu-id="3b265-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="3b265-176">Agora podemos modificar a lógica para retornar um objeto de álbum para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="3b265-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="3b265-177">Posteriormente no tutorial Vamos recuperar os dados de um banco de dados – mas para agora, usaremos "fictício dados" para começar a usar.</span><span class="sxs-lookup"><span data-stu-id="3b265-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="3b265-178">*Observação: Se você estiver familiarizado com c#, você pode supor que, usando var significa que nossa variável de álbum é a associação tardia. Não está correta – o compilador do c# é usando inferência de tipo com base no qual estamos atribuindo à variável para determinar o que álbum é do tipo álbum e compilando a variável local álbum como um tipo de álbum, portanto, obtemos a verificação de tempo de compilação e o editor de códigos do Visual Studio suporte.*</span><span class="sxs-lookup"><span data-stu-id="3b265-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="3b265-179">Agora, vamos criar um modelo de exibição que usa nosso álbum para gerar uma resposta HTML.</span><span class="sxs-lookup"><span data-stu-id="3b265-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="3b265-180">Antes de fazer isso é necessário compilar o projeto para que a caixa de diálogo Adicionar modo de exibição sabe sobre nossa classe de álbum recém-criado.</span><span class="sxs-lookup"><span data-stu-id="3b265-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="3b265-181">Você pode compilar o projeto, selecionando o Debug⇨Build MvcMusicStore item de menu (para créditos extras, você pode usar o atalho Ctrl-Shift-B para compilar o projeto).</span><span class="sxs-lookup"><span data-stu-id="3b265-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="3b265-182">Agora que configuramos nosso classes de suporte, estamos prontos para criar nosso modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="3b265-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="3b265-183">Clique com botão direito dentro do método detalhes e selecione "Adicionar modo de exibição..." no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="3b265-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="3b265-184">Vamos criar um novo modelo de exibição como fizemos anteriormente com o HomeController.</span><span class="sxs-lookup"><span data-stu-id="3b265-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="3b265-185">Como estamos criando do StoreController ele será por padrão gerado em um arquivo \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="3b265-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="3b265-186">Ao contrário de antes, vamos verificar a caixa de seleção "Criar um fortemente tipados" modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="3b265-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="3b265-187">Em seguida, vamos selecionar nossa classe de "Álbum" dentro de "Modo de exibição de dados-class" drop-downlist.</span><span class="sxs-lookup"><span data-stu-id="3b265-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="3b265-188">Isso fará com que a caixa de diálogo "Adicionar exibição" criar um modelo de exibição que espera que um álbum de objeto será passado a ele para usar.</span><span class="sxs-lookup"><span data-stu-id="3b265-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="3b265-189">Quando clicamos no botão "Adicionar" nosso modelo de exibição \Views\Store\Details.cshtml será criado, que contém o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="3b265-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="3b265-190">Observe que a primeira linha, que indica que essa exibição está fortemente tipada para nossa classe de álbum.</span><span class="sxs-lookup"><span data-stu-id="3b265-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="3b265-191">O mecanismo de exibição Razor entende que ele foi passado um objeto de álbum, podemos facilmente acessar as propriedades do modelo e até mesmo ter a vantagem do IntelliSense no editor do Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="3b265-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="3b265-192">Atualizar o &lt;h2&gt; Marque para que ele exibe a propriedade de título do álbum, modificando a linha a ser exibido da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="3b265-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="3b265-193">Observe que o IntelliSense é disparado quando você insere o período após o @Model palavra-chave, mostrando as propriedades e métodos que oferece suporte a classe de álbum.</span><span class="sxs-lookup"><span data-stu-id="3b265-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="3b265-194">Vamos agora executar novamente o nosso projeto e acesse a URL de detalhes/Store/5.</span><span class="sxs-lookup"><span data-stu-id="3b265-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="3b265-195">Vamos ver detalhes de um álbum, como abaixo.</span><span class="sxs-lookup"><span data-stu-id="3b265-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="3b265-196">Agora vamos fazer uma atualização semelhante para o método de ação Store procurar.</span><span class="sxs-lookup"><span data-stu-id="3b265-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="3b265-197">Atualize o método para que ela retorne um ActionResult e modificar a lógica do método, portanto, ele cria um novo objeto de gênero e retorna-o para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="3b265-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="3b265-198">Clique com botão direito no método procurar e selecione "Adicionar modo de exibição..." no menu de contexto, em seguida, adicione uma exibição que é fortemente tipada adicionar fortemente tipados para a classe de gênero.</span><span class="sxs-lookup"><span data-stu-id="3b265-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="3b265-199">Atualizar o &lt;h2&gt; elemento no modo de exibição de código (em /Views/Store/Browse.cshtml) para exibir as informações de gênero.</span><span class="sxs-lookup"><span data-stu-id="3b265-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="3b265-200">Agora vamos executar novamente o nosso projeto e navegue até/Store/procurar? Gênero = URL globo espelhado.</span><span class="sxs-lookup"><span data-stu-id="3b265-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="3b265-201">Veremos a página Procurar exibida abaixo da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="3b265-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="3b265-202">Por fim, vamos fazer uma atualização de um pouco mais complexa o **Store índice** exibição para exibir uma lista de todos os gêneros em nosso repositório e o método de ação.</span><span class="sxs-lookup"><span data-stu-id="3b265-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="3b265-203">Faremos isso usando uma lista de gêneros como nosso objeto de modelo, em vez de apenas um único gênero.</span><span class="sxs-lookup"><span data-stu-id="3b265-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="3b265-204">Clique com botão direito no método de ação de índice Store e selecione Adicionar exibição como antes, selecione o gênero como a classe de modelo e pressione o botão Adicionar.</span><span class="sxs-lookup"><span data-stu-id="3b265-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="3b265-205">Primeiro vamos alterar o @model declaração para indicar que o modo de exibição estará esperando gênero vários objetos em vez de apenas um.</span><span class="sxs-lookup"><span data-stu-id="3b265-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="3b265-206">Altere a primeira linha do /Store/Index.cshtml como segue:</span><span class="sxs-lookup"><span data-stu-id="3b265-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="3b265-207">Isso instrui o mecanismo de exibição do Razor que ele estará trabalhando com um objeto de modelo que pode conter vários objetos de gênero.</span><span class="sxs-lookup"><span data-stu-id="3b265-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="3b265-208">Estamos usando um IEnumerable&lt;gênero&gt; em vez de uma lista&lt;gênero&gt; , pois ele é mais genérico, que nos permite alterar nosso tipo de modelo mais tarde para qualquer tipo de objeto que dá suporte a interface IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="3b265-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="3b265-209">Em seguida, vamos fazer um loop por meio dos objetos de gênero no modelo, conforme mostrado no código de exibição completo abaixo.</span><span class="sxs-lookup"><span data-stu-id="3b265-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="3b265-210">Observe que temos suporte total ao IntelliSense como podemos inserir esse código, para que quando digitamos "@Model."</span><span class="sxs-lookup"><span data-stu-id="3b265-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="3b265-211">podemos ver todos os métodos e propriedades com suporte por um IEnumerable de tipo de gênero.</span><span class="sxs-lookup"><span data-stu-id="3b265-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="3b265-212">Dentro do nosso loop "foreach", o Visual Web Developer sabe que cada item é do tipo gênero, para que possamos ver IntelliSence para cada tipo de gênero.</span><span class="sxs-lookup"><span data-stu-id="3b265-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSence for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="3b265-213">Em seguida, o recurso de scaffolding examinado o objeto de gênero e determinado a cada uma terá uma propriedade de nome, para que ele percorre e grava-os. Ele também gera links de edição, detalhes e exclusão para cada item individual.</span><span class="sxs-lookup"><span data-stu-id="3b265-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="3b265-214">Podemos tirar vantagem que mais tarde no nosso gerente da loja, mas por enquanto gostaríamos de ter uma lista simples em vez disso.</span><span class="sxs-lookup"><span data-stu-id="3b265-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="3b265-215">Quando executar o aplicativo e navegue até /Store, podemos ver que a contagem e a lista de gêneros é exibida.</span><span class="sxs-lookup"><span data-stu-id="3b265-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="3b265-216">Adicionando Links entre as páginas</span><span class="sxs-lookup"><span data-stu-id="3b265-216">Adding Links between pages</span></span>

<span data-ttu-id="3b265-217">Nossa URL /Store que lista os gêneros atualmente lista os nomes de gênero simplesmente como texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="3b265-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="3b265-218">Vamos alterar isso para que, em vez de texto sem formatação em vez disso, temos o link de nomes de gênero para a URL do Store/procura apropriada, para que o clique em um gênero de música, como "Disco" navegará até/Store/procurar? genre = URL do Disco.</span><span class="sxs-lookup"><span data-stu-id="3b265-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="3b265-219">Podemos atualizar nosso modelo de exibição \Views\Store\Index.cshtml para esses links usando código como o abaixo de saída **(não digite isso em – vamos melhorá-lo)**:</span><span class="sxs-lookup"><span data-stu-id="3b265-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="3b265-220">Isso funciona, mas isso pode levar a problemas mais tarde, já que ele se baseia em uma cadeia de caracteres codificada.</span><span class="sxs-lookup"><span data-stu-id="3b265-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="3b265-221">Por exemplo, se quiséssemos renomear o controlador, podemos precisaria pesquisar por meio de nosso código procurando por links que precisam ser atualizados.</span><span class="sxs-lookup"><span data-stu-id="3b265-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="3b265-222">É uma abordagem alternativa, que podemos usar tirar proveito de um método auxiliar HTML.</span><span class="sxs-lookup"><span data-stu-id="3b265-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="3b265-223">O ASP.NET MVC inclui os métodos de auxiliares de HTML que estão disponíveis no nosso código de modelo de exibição para executar uma variedade de tarefas comuns, como isso.</span><span class="sxs-lookup"><span data-stu-id="3b265-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="3b265-224">O método auxiliar Html.ActionLink() é particularmente útil e facilita a compilação HTML &lt;um&gt; vincula e cuida dos detalhes desagradáveis como certificando-se de caminhos de URL sejam corretamente codificados de URL.</span><span class="sxs-lookup"><span data-stu-id="3b265-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="3b265-225">Html.ActionLink() tem várias sobrecargas diferentes para permitir a especificação tanta informação quanto você precisa para os links.</span><span class="sxs-lookup"><span data-stu-id="3b265-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="3b265-226">No caso mais simples, você irá fornecer apenas o texto do link e o método de ação para ir para quando o hiperlink é clicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="3b265-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="3b265-227">Por exemplo, podemos pode vincular a "/ Store /" método Index () na página de detalhes de Store com o texto do link de "Ir para a Store índice" usando a seguinte chamada:</span><span class="sxs-lookup"><span data-stu-id="3b265-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="3b265-228">*Observação: nesse caso, não precisamos especificar o nome do controlador porque estamos apenas estiver vinculando a outra ação dentro do mesmo controlador que está renderizando o modo de exibição atual.*</span><span class="sxs-lookup"><span data-stu-id="3b265-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="3b265-229">Nosso links para a página Procurar precisará passar um parâmetro, no entanto, então, vamos usar outra sobrecarga do método ActionLink que usa três parâmetros:</span><span class="sxs-lookup"><span data-stu-id="3b265-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="3b265-230">Texto do link, que exibirá o nome de gênero</span><span class="sxs-lookup"><span data-stu-id="3b265-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="3b265-231">Nome da ação de controlador (Procurar)</span><span class="sxs-lookup"><span data-stu-id="3b265-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="3b265-232">Valores de parâmetro de rota, especificando o nome (gênero) e o valor (nome de gênero)</span><span class="sxs-lookup"><span data-stu-id="3b265-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="3b265-233">Colocando-se de que tudo isso, aqui está como vamos escrever esses links para a exibição de índice Store:</span><span class="sxs-lookup"><span data-stu-id="3b265-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="3b265-234">Agora quando podemos executar nosso projeto novamente e acesse a URL /Store/ veremos uma lista de gêneros.</span><span class="sxs-lookup"><span data-stu-id="3b265-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="3b265-235">Cada gênero é um hiperlink – quando clicado ela nos levará à nossa/Store/procurar? genre =*[gênero]* URL.</span><span class="sxs-lookup"><span data-stu-id="3b265-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="3b265-236">O HTML para a lista de gênero tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="3b265-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> <span data-ttu-id="3b265-237">[Anterior](mvc-music-store-part-2.md)
> [Próximo](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="3b265-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>

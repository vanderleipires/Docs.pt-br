---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: "Parte 3: Exibições e ViewModels | Microsoft Docs"
author: jongalloway
description: "Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 3 abrange ViewModels e exibições."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 5b38f88283c5d2d93f0bab283dbd9451855d95e3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="e1395-104">Parte 3: Exibições e ViewModels</span><span class="sxs-lookup"><span data-stu-id="e1395-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="e1395-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="e1395-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="e1395-106">O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.</span><span class="sxs-lookup"><span data-stu-id="e1395-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="e1395-107">O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="e1395-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="e1395-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e1395-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="e1395-109">Parte 3 abrange ViewModels e exibições.</span><span class="sxs-lookup"><span data-stu-id="e1395-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="e1395-110">Até agora é tiver apenas foi retornar cadeias de caracteres de ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="e1395-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="e1395-111">Que é uma boa forma de obter uma ideia de como funcionam os controladores, mas não como você seria deseja criar um aplicativo web real.</span><span class="sxs-lookup"><span data-stu-id="e1395-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="e1395-112">Vamos deseja uma maneira melhor de gerar HTML para navegadores visitar nosso site – um em que podemos usar arquivos de modelo para personalizar mais facilmente o conteúdo HTML enviada de volta.</span><span class="sxs-lookup"><span data-stu-id="e1395-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="e1395-113">É exatamente o que fazem modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="e1395-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="e1395-114">Adicionando um modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="e1395-114">Adding a View template</span></span>

<span data-ttu-id="e1395-115">Para usar um modelo de exibição, vamos alterar o método HomeController índice para retornar um ActionResult e ainda retornar View(), como abaixo:</span><span class="sxs-lookup"><span data-stu-id="e1395-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="e1395-116">A alteração acima indica que, em vez de retornou uma cadeia de caracteres, em vez disso, queremos usar uma "exibição" para gerar um resultado de volta.</span><span class="sxs-lookup"><span data-stu-id="e1395-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="e1395-117">Agora vamos adicionar um modelo de exibição apropriado ao nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="e1395-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="e1395-118">Para fazer isso, vamos posicionar o cursor de texto dentro do método de ação do índice, e em seguida, clique com botão direito e selecione "Adicionar modo de exibição".</span><span class="sxs-lookup"><span data-stu-id="e1395-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="e1395-119">Isso abrirá a caixa de diálogo Adicionar modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="e1395-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="e1395-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e1395-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="e1395-121">A caixa de diálogo "Adicionar modo de exibição" permite a rápida e facilmente gerar arquivos de modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="e1395-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="e1395-122">Por padrão "Adicionar modo de exibição" caixa de diálogo popula previamente o nome do modelo de exibição para criar exatamente como o método de ação que irá usá-la.</span><span class="sxs-lookup"><span data-stu-id="e1395-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="e1395-123">Porque usamos o menu de contexto "Adicionar modo de exibição" dentro do método de ação Index () do nosso HomeController, a caixa de diálogo "Adicionar modo de exibição" acima tem "Index" como o nome de exibição preenchido por padrão.</span><span class="sxs-lookup"><span data-stu-id="e1395-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="e1395-124">Não precisamos alterar qualquer uma das opções na caixa de diálogo, portanto, clique no botão Adicionar.</span><span class="sxs-lookup"><span data-stu-id="e1395-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="e1395-125">Depois, clique no botão Adicionar, Visual Web Developer criará um cshtml novo modelo de exibição para nós no diretório \Views\Home, criando a pasta se já não existe.</span><span class="sxs-lookup"><span data-stu-id="e1395-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="e1395-126">O nome e uma pasta local do arquivo "Cshtml" é importante e segue as convenções de nomenclatura padrão ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e1395-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="e1395-127">O nome do diretório, \Views\Home, coincide com o controlador - que é chamado HomeController.</span><span class="sxs-lookup"><span data-stu-id="e1395-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="e1395-128">Nome do modelo da exibição, índice, faz a correspondência de método de ação do controlador que deseja exibir o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="e1395-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="e1395-129">ASP.NET MVC permite evitar ter que especificar explicitamente o nome ou local de um modelo de exibição quando usamos essa convenção de nomenclatura para retornar uma exibição.</span><span class="sxs-lookup"><span data-stu-id="e1395-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="e1395-130">Ele por padrão processará o modelo de exibição \Views\Home\Index.cshtml quando escrevemos código como abaixo em nosso HomeController:</span><span class="sxs-lookup"><span data-stu-id="e1395-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="e1395-131">O Visual Web Developer criado e aberto o modelo de exibição "Cshtml" depois de clicar no botão "Adicionar" na caixa de diálogo "Adicionar modo de exibição".</span><span class="sxs-lookup"><span data-stu-id="e1395-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="e1395-132">O conteúdo de cshtml é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="e1395-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="e1395-133">Essa exibição está usando a sintaxe do Razor, que é mais concisa que o mecanismo de exibição de Web Forms usado em Web Forms do ASP.NET e versões anteriores do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e1395-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="e1395-134">O mecanismo de exibição de Web Forms ainda está disponível no ASP.NET MVC 3, mas muitos desenvolvedores consideram que o mecanismo de exibição Razor adequada o desenvolvimento do ASP.NET MVC muito bem.</span><span class="sxs-lookup"><span data-stu-id="e1395-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="e1395-135">As três primeiras linhas definem o título da página usando ViewBag.</span><span class="sxs-lookup"><span data-stu-id="e1395-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="e1395-136">Vamos examinar como isso funciona em mais detalhes em breve, mas primeiro vamos atualizar o texto de cabeçalho de texto e exibir a página.</span><span class="sxs-lookup"><span data-stu-id="e1395-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="e1395-137">Atualização de &lt;h2&gt; marca dizer "Esta é a Home Page", conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="e1395-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="e1395-138">Executando a aplicativo mostra que o nosso novo texto está visível na página inicial.</span><span class="sxs-lookup"><span data-stu-id="e1395-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="e1395-139">Usando um Layout para elementos comuns do site</span><span class="sxs-lookup"><span data-stu-id="e1395-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="e1395-140">A maioria dos sites têm conteúdo que é compartilhado entre várias páginas: navegação rodapés, imagens de logotipo, referências de folha de estilos, etc. O mecanismo de exibição Razor torna isso fácil de gerenciar usando uma página chamada \_cshtml que automaticamente foi criado para nós dentro da pasta/exibições/compartilhado.</span><span class="sxs-lookup"><span data-stu-id="e1395-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="e1395-141">Clique duas vezes nessa pasta para exibir o conteúdo, que são mostrados abaixo.</span><span class="sxs-lookup"><span data-stu-id="e1395-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="e1395-142">O conteúdo do nosso exibições individuais será exibido pelo @RenderBodycomando () e qualquer conteúdo comum que queremos aparecer fora do que podem ser adicionados para o \_cshtml marcação.</span><span class="sxs-lookup"><span data-stu-id="e1395-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="e1395-143">Queremos será nosso repositório de música MVC para tiver um cabeçalho de comuns com links para a área página inicial e o armazenamento em todas as páginas do site, portanto, adicionaremos que o modelo diretamente acima que @RenderBodyinstrução ().</span><span class="sxs-lookup"><span data-stu-id="e1395-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="e1395-144">Atualizar a folha de estilos</span><span class="sxs-lookup"><span data-stu-id="e1395-144">Updating the StyleSheet</span></span>

<span data-ttu-id="e1395-145">O modelo de projeto vazio inclui um arquivo CSS muito simplificado que inclui apenas os estilos usados para exibir mensagens de validação.</span><span class="sxs-lookup"><span data-stu-id="e1395-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="e1395-146">Nosso designer fornece alguns CSS e imagens adicionais para definir a aparência do nosso site, portanto, adicionaremos os agora.</span><span class="sxs-lookup"><span data-stu-id="e1395-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="e1395-147">O arquivo CSS atualizado e as imagens são incluídas no diretório de conteúdo de Assets.zip MvcMusicStore que está disponível em [repositório de música MVC](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="e1395-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="e1395-148">Vamos selecionar ambos no Windows Explorer e soltá-los na pasta de conteúdo da nossa solução no Visual Web Developer, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="e1395-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="e1395-149">Você será solicitado a confirmar se deseja substituir o arquivo de site existente.</span><span class="sxs-lookup"><span data-stu-id="e1395-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="e1395-150">Clique em Sim.</span><span class="sxs-lookup"><span data-stu-id="e1395-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="e1395-151">A pasta de conteúdo do seu aplicativo agora será exibida da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="e1395-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="e1395-152">Agora vamos executar o aplicativo e ver a aparecem de nossas alterações na Home page.</span><span class="sxs-lookup"><span data-stu-id="e1395-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="e1395-153">Vamos analisar o que mudou: método de ação do HomeController o índice encontrado e exibidos modelo \Views\Home\Index.cshtmlView, embora nosso código chamado "retorno View()", pois nosso modelo de exibição seguido a convenção de nomenclatura padrão.</span><span class="sxs-lookup"><span data-stu-id="e1395-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="e1395-154">A Home Page está exibindo uma mensagem de boas-vinda simples que é definida dentro do modelo de exibição \Views\Home\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="e1395-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="e1395-155">A Home Page está usando nosso \_cshtml modelo, e, portanto, a mensagem de boas-vinda está contida dentro do layout do site padrão HTML.</span><span class="sxs-lookup"><span data-stu-id="e1395-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="e1395-156">Usando um modelo para passar informações para nossa visão</span><span class="sxs-lookup"><span data-stu-id="e1395-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="e1395-157">Um modelo de exibição que exibe apenas inserido no código HTML não vai fazer um site da web muito interessantes.</span><span class="sxs-lookup"><span data-stu-id="e1395-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="e1395-158">Para criar um site da web dinâmico, em vez disso, podemos desejará passar informações de nosso ações do controlador para os nossos modelos de exibição.</span><span class="sxs-lookup"><span data-stu-id="e1395-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="e1395-159">O padrão Model-View-Controller, o termo A que modelo refere-se objetos que representam os dados no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e1395-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="e1395-160">Geralmente, objetos de modelo correspondem às tabelas no banco de dados, mas não é necessário.</span><span class="sxs-lookup"><span data-stu-id="e1395-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="e1395-161">Métodos de ação do controlador que retornam um ActionResult podem passar um objeto de modelo para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="e1395-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="e1395-162">Isso permite que um controlador pacote corretamente todas as informações necessárias para gerar uma resposta e, em seguida, passar essa informação para um modelo de exibição a ser usado para gerar a resposta HTML apropriada.</span><span class="sxs-lookup"><span data-stu-id="e1395-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="e1395-163">Isso é mais fácil de entender, vê-lo em ação, vamos começar.</span><span class="sxs-lookup"><span data-stu-id="e1395-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="e1395-164">Primeiro, vamos criar algumas classes de modelo para representar gêneros e álbuns em nosso repositório.</span><span class="sxs-lookup"><span data-stu-id="e1395-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="e1395-165">Vamos começar criando uma classe de gênero.</span><span class="sxs-lookup"><span data-stu-id="e1395-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="e1395-166">Clique na pasta "Modelos" dentro de seu projeto, escolha a opção "Adicionar classe" e nomeie o arquivo "Genre.cs".</span><span class="sxs-lookup"><span data-stu-id="e1395-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="e1395-167">Em seguida, adicione uma propriedade de nome da cadeia de caracteres pública à classe que foi criada:</span><span class="sxs-lookup"><span data-stu-id="e1395-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="e1395-168">*Observação: no caso você esteja se perguntando, o {get; definir;} notação está fazendo uso do recurso de Propriedades autoimplementadas do #. Isso nos fornece os benefícios de uma propriedade sem a necessidade de nós declarar um campo existente.*</span><span class="sxs-lookup"><span data-stu-id="e1395-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="e1395-169">Em seguida, siga as mesmas etapas para criar uma classe álbum (chamada Album.cs) que tem um título e uma propriedade de Gênero:</span><span class="sxs-lookup"><span data-stu-id="e1395-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="e1395-170">Agora podemos modificar StoreController para usar os modos de exibição que exibem informações dinâmicas do nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="e1395-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="e1395-171">Se - - agora para fins de demonstração é denominado nossos álbuns com base na ID da solicitação, podemos pode exibir essas informações como o modo de exibição abaixo.</span><span class="sxs-lookup"><span data-stu-id="e1395-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="e1395-172">Vamos começar alterando a ação de detalhes do repositório para que ele mostra as informações para um único álbum.</span><span class="sxs-lookup"><span data-stu-id="e1395-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="e1395-173">Adicione uma instrução "using" na parte superior do **StoreControllers** classe para incluir o namespace MvcMusicStore.Models, portanto, não precisamos digite MvcMusicStore.Models.Album toda vez que você deseja usar a classe álbum.</span><span class="sxs-lookup"><span data-stu-id="e1395-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="e1395-174">A seção "usos" da classe agora deve aparecer abaixo.</span><span class="sxs-lookup"><span data-stu-id="e1395-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="e1395-175">Em seguida, vamos atualizar a ação de controlador de detalhes para que ela retorne um ActionResult em vez de uma cadeia de caracteres, como foi feito com o método de índice do HomeController.</span><span class="sxs-lookup"><span data-stu-id="e1395-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="e1395-176">Agora podemos modificar a lógica para retornar um objeto álbum para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="e1395-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="e1395-177">Posteriormente neste tutorial Vamos recuperar os dados de um banco de dados – mas para agora usaremos "fictício dados" para começar.</span><span class="sxs-lookup"><span data-stu-id="e1395-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="e1395-178">*Observação: Se você estiver familiarizado com c#, você pode presumir que usar var significa que nossa variável álbum associação tardia. Não é correto – o compilador c# é usando inferência de tipo com base no qual estamos atribuir à variável para determinar o que álbum é do tipo álbum e compilar a variável local álbum como um tipo de álbum, portanto obtemos a verificação de tempo de compilação e o editor de códigos do Visual Studio suporte.*</span><span class="sxs-lookup"><span data-stu-id="e1395-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="e1395-179">Agora vamos criar um modelo de exibição que usa o nosso álbum para gerar uma resposta HTML.</span><span class="sxs-lookup"><span data-stu-id="e1395-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="e1395-180">Antes de fazermos isso, precisamos criar o projeto para que a caixa de diálogo Adicionar modo de exibição sabe sobre nossa classe álbum recém-criado.</span><span class="sxs-lookup"><span data-stu-id="e1395-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="e1395-181">Você pode compilar o projeto selecionando o Debug⇨Build MvcMusicStore item de menu (para crédito extra, você pode usar o atalho Ctrl-Shift-B para compilar o projeto).</span><span class="sxs-lookup"><span data-stu-id="e1395-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="e1395-182">Agora que criamos nossas classes de suporte, estamos prontos para compilar nosso modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="e1395-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="e1395-183">Clique no método de detalhes e selecione "Adicionar modo de exibição..."</span><span class="sxs-lookup"><span data-stu-id="e1395-183">Right-click within the Details method and select "Add View…"</span></span> <span data-ttu-id="e1395-184">no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="e1395-184">from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="e1395-185">Vamos criar um novo modelo de exibição como antes com HomeController.</span><span class="sxs-lookup"><span data-stu-id="e1395-185">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="e1395-186">Porque estamos criando do StoreController será por padrão gerado em um arquivo \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="e1395-186">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="e1395-187">Ao contrário, vamos para verificar se a caixa de seleção "Criar fortemente tipado" modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="e1395-187">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="e1395-188">Em seguida, vamos selecionar nossa classe de "Álbum" dentro de "Classe de dados de exibição" drop-downlist.</span><span class="sxs-lookup"><span data-stu-id="e1395-188">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="e1395-189">Isso fará com que a caixa de diálogo "Adicionar modo de exibição" criar um modelo de exibição que espera que um álbum objeto será passado para ele usar.</span><span class="sxs-lookup"><span data-stu-id="e1395-189">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="e1395-190">Quando, clique no botão "Adicionar" nosso modelo de exibição \Views\Store\Details.cshtml será criado, que contém o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e1395-190">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="e1395-191">Observe que a primeira linha, o que indica que essa exibição está fortemente tipado para nossa classe álbum.</span><span class="sxs-lookup"><span data-stu-id="e1395-191">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="e1395-192">O mecanismo de exibição Razor compreende que ele foi passado um objeto álbum, podemos acessar facilmente as propriedades de modelo e ter o benefício do IntelliSense no editor do Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="e1395-192">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="e1395-193">Atualização de &lt;h2&gt; marcar exibe propriedades do título do álbum, modificando a linha da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="e1395-193">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="e1395-194">Observe que o IntelliSense é acionado quando você insere o período após o @Model palavra-chave, mostrando as propriedades e métodos que oferece suporte a classe álbum.</span><span class="sxs-lookup"><span data-stu-id="e1395-194">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="e1395-195">Vamos agora execute novamente o nosso projeto e visita a URL de repositório/detalhes/5.</span><span class="sxs-lookup"><span data-stu-id="e1395-195">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="e1395-196">Veremos detalhes de um álbum como abaixo.</span><span class="sxs-lookup"><span data-stu-id="e1395-196">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="e1395-197">Agora será feita uma atualização semelhante para o método de ação procurar armazenamento.</span><span class="sxs-lookup"><span data-stu-id="e1395-197">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="e1395-198">O método de atualização para que ela retorne um ActionResult e modificar a lógica do método para que ele cria um novo objeto de gênero e retorna ao modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="e1395-198">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="e1395-199">Clique com botão direito no método procurar e selecione "Adicionar modo de exibição..."</span><span class="sxs-lookup"><span data-stu-id="e1395-199">Right-click in the Browse method and select "Add View…"</span></span> <span data-ttu-id="e1395-200">no menu de contexto, em seguida, adicione uma exibição fortemente tipada adicionar fortemente tipada para a classe de gênero.</span><span class="sxs-lookup"><span data-stu-id="e1395-200">from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="e1395-201">Atualização de &lt;h2&gt; elemento no modo de exibição de código (em /Views/Store/Browse.cshtml) para exibir as informações de gênero.</span><span class="sxs-lookup"><span data-stu-id="e1395-201">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="e1395-202">Agora vamos executar novamente o nosso projeto e navegue até o repositório/procurar? Gênero = URL globo.</span><span class="sxs-lookup"><span data-stu-id="e1395-202">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="e1395-203">Veremos a página Procurar exibida como abaixo.</span><span class="sxs-lookup"><span data-stu-id="e1395-203">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="e1395-204">Por fim, vamos fazer uma atualização de um pouco mais complexa para o **índice armazenar** método de ação e o modo de exibição para exibir uma lista de todos os gêneros na loja.</span><span class="sxs-lookup"><span data-stu-id="e1395-204">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="e1395-205">Faremos isso usando uma lista de gêneros como nosso objeto de modelo, em vez de apenas um único gênero.</span><span class="sxs-lookup"><span data-stu-id="e1395-205">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="e1395-206">Clique no método de ação de índice de repositório e selecione Adicionar modo de exibição como antes, selecione gênero, como a classe de modelo e pressione o botão Adicionar.</span><span class="sxs-lookup"><span data-stu-id="e1395-206">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="e1395-207">Primeiro vamos alterar o @model declaração para indicar que o modo de exibição estará esperando gênero vários objetos em vez de apenas um.</span><span class="sxs-lookup"><span data-stu-id="e1395-207">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="e1395-208">Altere a primeira linha do /Store/Index.cshtml da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="e1395-208">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="e1395-209">Isso informa ao mecanismo de exibição do Razor que estará trabalhando com um objeto de modelo que pode conter vários objetos de gênero.</span><span class="sxs-lookup"><span data-stu-id="e1395-209">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="e1395-210">Estamos usando um IEnumerable&lt;gênero&gt; em vez de uma lista&lt;gênero&gt; como ele é mais genérico, permitindo-nos alterar os tipo de modelo mais tarde para qualquer tipo de objeto que oferece suporte a interface IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="e1395-210">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="e1395-211">Em seguida, vamos será loop pelos objetos no modelo, conforme mostrado no código abaixo concluídas gênero.</span><span class="sxs-lookup"><span data-stu-id="e1395-211">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="e1395-212">Observe que há total suporte do IntelliSense como podemos inserir esse código, para que quando digitamos "@Model."</span><span class="sxs-lookup"><span data-stu-id="e1395-212">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="e1395-213">podemos ver todos os métodos e propriedades com suporte por um IEnumerable de tipo gênero.</span><span class="sxs-lookup"><span data-stu-id="e1395-213">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="e1395-214">Em nosso loop "foreach", Visual Web Developer sabe que cada item é do tipo gênero, para vemos IntelliSence para cada tipo de gênero.</span><span class="sxs-lookup"><span data-stu-id="e1395-214">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSence for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="e1395-215">Em seguida, o recurso de scaffolding examinou o objeto de gênero e determinado que cada uma terá uma propriedade de nome, para que ele percorre e grava-los. Ela também gera links de edição, detalhes e exclusão para cada item individual.</span><span class="sxs-lookup"><span data-stu-id="e1395-215">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="e1395-216">Vamos vantagem de que mais tarde em nossa gerente da loja, mas agora queremos ter uma lista simples em vez disso.</span><span class="sxs-lookup"><span data-stu-id="e1395-216">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="e1395-217">Quando executar o aplicativo e navegue até /Store, podemos ver que a contagem e a lista de gêneros é exibido.</span><span class="sxs-lookup"><span data-stu-id="e1395-217">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="e1395-218">Adicionando Links entre páginas</span><span class="sxs-lookup"><span data-stu-id="e1395-218">Adding Links between pages</span></span>

<span data-ttu-id="e1395-219">Nossa URL /Store que lista gêneros atualmente lista os nomes de gênero simplesmente como texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="e1395-219">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="e1395-220">Vamos alterar isso para que, em vez de texto sem formatação em vez disso, temos o link de nomes de gênero para a URL de repositório/procurar apropriada, para que clicar em um gênero de música, como "Disco" navegará para a loja/procurar? gênero = URL do Disco.</span><span class="sxs-lookup"><span data-stu-id="e1395-220">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="e1395-221">Foi atualizamos nosso modelo de exibição de \Views\Store\Index.cshtml à saída desses links usando código como abaixo **(não digite o texto, vamos melhorá-lo)**:</span><span class="sxs-lookup"><span data-stu-id="e1395-221">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="e1395-222">Isso funciona, mas isso pode levar a problemas mais tarde, desde que ele se baseia em uma cadeia de caracteres codificada.</span><span class="sxs-lookup"><span data-stu-id="e1395-222">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="e1395-223">Por exemplo, se quiséssemos renomear o controlador, podemos precisa pesquisar por meio de nosso código procurando links que precisam ser atualizados.</span><span class="sxs-lookup"><span data-stu-id="e1395-223">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="e1395-224">É uma abordagem alternativa que podemos usar tirar proveito de um método auxiliar HTML.</span><span class="sxs-lookup"><span data-stu-id="e1395-224">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="e1395-225">O ASP.NET MVC inclui os métodos auxiliares HTML que estão disponíveis em nosso código de modelo de exibição para executar uma variedade de tarefas comuns como isso.</span><span class="sxs-lookup"><span data-stu-id="e1395-225">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="e1395-226">O método auxiliar Html.ActionLink() é particularmente útil e torna mais fácil criar HTML &lt;um&gt; links e cuida dos detalhes irritantes, como verificar se os caminhos de URL corretamente são codificados de URL.</span><span class="sxs-lookup"><span data-stu-id="e1395-226">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="e1395-227">Html.ActionLink() tem várias sobrecargas diferentes para permitir a especificação de informações conforme necessário para os links.</span><span class="sxs-lookup"><span data-stu-id="e1395-227">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="e1395-228">No caso mais simples, você irá fornecer apenas o texto do link e o método de ação para ir para quando o hiperlink é clicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="e1395-228">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="e1395-229">Por exemplo, podemos pode vincular a "Repositório /" método Index () na página de detalhes do repositório com o texto do link "Ir para o índice de repositório" usando a chamada a seguir:</span><span class="sxs-lookup"><span data-stu-id="e1395-229">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="e1395-230">*Observação: nesse caso, não precisamos especificar o nome do controlador porque estamos apenas está vinculando outra ação no mesmo controlador que está processando o modo de exibição atual.*</span><span class="sxs-lookup"><span data-stu-id="e1395-230">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="e1395-231">Os links para a página Procurar precisará passar um parâmetro, porém, então, vamos usar outra sobrecarga do método ActionLink que usa três parâmetros:</span><span class="sxs-lookup"><span data-stu-id="e1395-231">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="e1395-232">Texto do link, que exibe o nome de gênero</span><span class="sxs-lookup"><span data-stu-id="e1395-232">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="e1395-233">Nome de ação do controlador (Procurar)</span><span class="sxs-lookup"><span data-stu-id="e1395-233">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="e1395-234">Valores de parâmetro de rota, especificando o nome (gênero) e o valor (nome de gênero)</span><span class="sxs-lookup"><span data-stu-id="e1395-234">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="e1395-235">Colocando-se de que todos os aspectos juntos, aqui está como vamos gravar esses links para a exibição do índice de repositório:</span><span class="sxs-lookup"><span data-stu-id="e1395-235">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="e1395-236">Agora quando executar o nosso projeto novamente e acessar a URL /Store/ veremos uma lista de gêneros.</span><span class="sxs-lookup"><span data-stu-id="e1395-236">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="e1395-237">Cada gênero é um hiperlink – quando clicado levará conosco para nosso repositório/procurar? gênero =*[gênero]* URL.</span><span class="sxs-lookup"><span data-stu-id="e1395-237">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="e1395-238">O HTML para a lista de gênero tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="e1395-238">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


>[!div class="step-by-step"]
<span data-ttu-id="e1395-239">[Anterior](mvc-music-store-part-2.md)
[Próximo](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="e1395-239">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>

---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: Controladores | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 2 abrange controladores.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 680cdea388d9b01961bd626643c0fd91c9205ed7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="part-2-controllers"></a><span data-ttu-id="8edd9-104">Parte 2: controladores</span><span class="sxs-lookup"><span data-stu-id="8edd9-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="8edd9-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="8edd9-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="8edd9-106">O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.</span><span class="sxs-lookup"><span data-stu-id="8edd9-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="8edd9-107">O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="8edd9-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="8edd9-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8edd9-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="8edd9-109">Parte 2 abrange controladores.</span><span class="sxs-lookup"><span data-stu-id="8edd9-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="8edd9-110">Estruturas da web tradicional, URLs de entrada geralmente são mapeados para arquivos no disco.</span><span class="sxs-lookup"><span data-stu-id="8edd9-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="8edd9-111">Por exemplo: uma solicitação para uma URL, como "/ products. aspx" ou "/ Products.php" pode ser processado por um arquivo de "Products. aspx" ou "Products.php".</span><span class="sxs-lookup"><span data-stu-id="8edd9-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="8edd9-112">Estruturas MVC baseado na Web mapeiam URLs para o código de servidor de forma ligeiramente diferente.</span><span class="sxs-lookup"><span data-stu-id="8edd9-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="8edd9-113">Em vez de mapeamento de URLs de entrada aos arquivos, em vez disso, eles são mapeados URLs para métodos nas classes.</span><span class="sxs-lookup"><span data-stu-id="8edd9-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="8edd9-114">Essas classes são chamadas de "Controladores" e eles são responsáveis pelo processamento de solicitações HTTP de entrada, manipulação de entrada do usuário, recuperar e salvar dados e determinar a resposta para enviar de volta ao cliente (exibição de HTML, baixar um arquivo, Redirecione para outro URL, etc.).</span><span class="sxs-lookup"><span data-stu-id="8edd9-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="8edd9-115">Adicionando um HomeController</span><span class="sxs-lookup"><span data-stu-id="8edd9-115">Adding a HomeController</span></span>

<span data-ttu-id="8edd9-116">Vamos começar nosso aplicativo de repositório de música MVC adicionando uma classe de controlador que tratará as URLs para a Home page do nosso site.</span><span class="sxs-lookup"><span data-stu-id="8edd9-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="8edd9-117">Vamos seguem as convenções de nomenclatura padrão do ASP.NET MVC e chamá-lo HomeController.</span><span class="sxs-lookup"><span data-stu-id="8edd9-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="8edd9-118">Clique na pasta "Controladores" no Gerenciador de soluções e selecione "Adicionar" e "Controlador …"</span><span class="sxs-lookup"><span data-stu-id="8edd9-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…"</span></span> <span data-ttu-id="8edd9-119">comando:</span><span class="sxs-lookup"><span data-stu-id="8edd9-119">command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="8edd9-120">Isso abrirá a caixa de diálogo "Adicionar controlador".</span><span class="sxs-lookup"><span data-stu-id="8edd9-120">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="8edd9-121">Nome do controlador "HomeController" e pressione o botão Adicionar.</span><span class="sxs-lookup"><span data-stu-id="8edd9-121">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="8edd9-122">Isso criará um novo arquivo, HomeController, com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="8edd9-122">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="8edd9-123">Para iniciar da maneira mais simples possível, vamos substituir o método de índice com um método simples que retorna apenas uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="8edd9-123">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="8edd9-124">Verifique duas alterações:</span><span class="sxs-lookup"><span data-stu-id="8edd9-124">We'll make two changes:</span></span>

- <span data-ttu-id="8edd9-125">Alterar o método para retornar uma cadeia de caracteres em vez de um ActionResult</span><span class="sxs-lookup"><span data-stu-id="8edd9-125">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="8edd9-126">Alterar a instrução return para retornar "Hello de início"</span><span class="sxs-lookup"><span data-stu-id="8edd9-126">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="8edd9-127">Agora, o método deve ser assim:</span><span class="sxs-lookup"><span data-stu-id="8edd9-127">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="8edd9-128">Executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="8edd9-128">Running the Application</span></span>

<span data-ttu-id="8edd9-129">Agora vamos executar o site.</span><span class="sxs-lookup"><span data-stu-id="8edd9-129">Now let's run the site.</span></span> <span data-ttu-id="8edd9-130">Podemos começar nosso servidor web e experimentar o site usando qualquer um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="8edd9-130">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="8edd9-131">Escolha o item de menu Iniciar depuração de depuração ⇨</span><span class="sxs-lookup"><span data-stu-id="8edd9-131">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="8edd9-132">Clique no botão de seta verde na barra de ferramentas ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="8edd9-132">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="8edd9-133">Use o atalho de teclado F5.</span><span class="sxs-lookup"><span data-stu-id="8edd9-133">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="8edd9-134">Usar qualquer uma das etapas acima compilar nosso projeto e, em seguida, fazer com que o ASP.NET Development Server é integrado ao Visual Web Developer para iniciar.</span><span class="sxs-lookup"><span data-stu-id="8edd9-134">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="8edd9-135">Uma notificação será exibida no canto inferior da tela para indicar que o servidor de desenvolvimento do ASP.NET foi iniciado e mostrará o número da porta que está sendo executado em.</span><span class="sxs-lookup"><span data-stu-id="8edd9-135">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="8edd9-136">O Visual Web Developer abrirá automaticamente uma janela do navegador cuja URL aponta para o nosso servidor web.</span><span class="sxs-lookup"><span data-stu-id="8edd9-136">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="8edd9-137">Isso nos permitirá testar rapidamente o nosso aplicativo web:</span><span class="sxs-lookup"><span data-stu-id="8edd9-137">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="8edd9-138">Okey, que era muito rápido – criamos um novo site, adicionado a uma função de três linha, e temos o texto em um navegador.</span><span class="sxs-lookup"><span data-stu-id="8edd9-138">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="8edd9-139">Não rocket ciência, mas é um começo.</span><span class="sxs-lookup"><span data-stu-id="8edd9-139">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="8edd9-140">*Observação: O Visual Web Developer inclui o ASP.NET Development Server, que irá executar o seu site em um número aleatório livre "porta". Na captura de tela acima, o site está em execução no `http://localhost:26641/`, de modo que ele está usando a porta 26641. O número da porta será diferente. Quando falamos sobre /Store/Browse like da URL neste tutorial, que irá após o número da porta. Supondo que um número de porta de 26641, navegando para armazenamento/procurar significa navegando para `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="8edd9-140">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="8edd9-141">Adicionando um StoreController</span><span class="sxs-lookup"><span data-stu-id="8edd9-141">Adding a StoreController</span></span>

<span data-ttu-id="8edd9-142">Adicionamos um HomeController simple que implementa a Home Page do nosso site.</span><span class="sxs-lookup"><span data-stu-id="8edd9-142">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="8edd9-143">Agora vamos adicionar outro controlador que usaremos para implementar a funcionalidade de navegação do nosso repositório de música.</span><span class="sxs-lookup"><span data-stu-id="8edd9-143">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="8edd9-144">Controlador nosso repositório oferecerá suporte a três cenários:</span><span class="sxs-lookup"><span data-stu-id="8edd9-144">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="8edd9-145">Uma página de listagem de gêneros de música em nosso repositório de música</span><span class="sxs-lookup"><span data-stu-id="8edd9-145">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="8edd9-146">Uma página de pesquisa que lista todos os álbuns de música em um gênero específico</span><span class="sxs-lookup"><span data-stu-id="8edd9-146">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="8edd9-147">Uma página de detalhes que mostra informações sobre um álbum de música específico</span><span class="sxs-lookup"><span data-stu-id="8edd9-147">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="8edd9-148">Vamos começar adicionando uma nova classe StoreController...</span><span class="sxs-lookup"><span data-stu-id="8edd9-148">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="8edd9-149">Se você ainda não fez isso, param o aplicativo fechar o navegador ou selecionando o item de menu de parar depuração ⇨ de depuração.</span><span class="sxs-lookup"><span data-stu-id="8edd9-149">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="8edd9-150">Agora, adicione um novo StoreController.</span><span class="sxs-lookup"><span data-stu-id="8edd9-150">Now add a new StoreController.</span></span> <span data-ttu-id="8edd9-151">Como com HomeController, faremos isso clicando duas vezes na pasta "Controladores" no Gerenciador de soluções e escolhendo Adicionar -&gt;item de menu do controlador</span><span class="sxs-lookup"><span data-stu-id="8edd9-151">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="8edd9-152">Nosso novo StoreController já tem um método "Index".</span><span class="sxs-lookup"><span data-stu-id="8edd9-152">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="8edd9-153">Vamos usar esse método "Index" implementar nossa página de listagem que lista todos os gêneros em nosso repositório de música.</span><span class="sxs-lookup"><span data-stu-id="8edd9-153">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="8edd9-154">Nós adicionaremos dois métodos adicionais para implementar os dois outros cenários queremos que nossa StoreController tratar: Procurar e detalhes.</span><span class="sxs-lookup"><span data-stu-id="8edd9-154">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="8edd9-155">Esses métodos (índice, procurar e detalhes) em nosso controlador são chamados de "Ações do controlador", e como você já viu com o método de ação HomeController.Index (), seu trabalho é responder às solicitações de URL e (geral) determinar qual conteúdo deve ser enviada de volta para o navegador ou o usuário que invocou a URL.</span><span class="sxs-lookup"><span data-stu-id="8edd9-155">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="8edd9-156">Vamos começar nosso implementação StoreController alterando theIndex() método para retornar a cadeia de caracteres "Hello de Store.Index()" e vamos adicionar métodos semelhantes para Browse () e Details():</span><span class="sxs-lookup"><span data-stu-id="8edd9-156">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="8edd9-157">Execute o projeto novamente e procurar as seguintes URLs:</span><span class="sxs-lookup"><span data-stu-id="8edd9-157">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="8edd9-158">/ Store</span><span class="sxs-lookup"><span data-stu-id="8edd9-158">/Store</span></span>
- <span data-ttu-id="8edd9-159">/ Repositório/procurar</span><span class="sxs-lookup"><span data-stu-id="8edd9-159">/Store/Browse</span></span>
- <span data-ttu-id="8edd9-160">/ / Detalhes do repositório</span><span class="sxs-lookup"><span data-stu-id="8edd9-160">/Store/Details</span></span>

<span data-ttu-id="8edd9-161">Acessar essas URLs invocar os métodos de ação em nosso controlador e retorna respostas de cadeia de caracteres:</span><span class="sxs-lookup"><span data-stu-id="8edd9-161">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="8edd9-162">Isso é ótimo, mas são cadeias de caracteres constantes apenas.</span><span class="sxs-lookup"><span data-stu-id="8edd9-162">That's great, but these are just constant strings.</span></span> <span data-ttu-id="8edd9-163">Vamos torná-los dinâmicos, para que eles levar informações da URL e exibem-lo na saída da página.</span><span class="sxs-lookup"><span data-stu-id="8edd9-163">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="8edd9-164">Primeiro, vamos alterar o método de ação de navegação para recuperar um valor de querystring da URL.</span><span class="sxs-lookup"><span data-stu-id="8edd9-164">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="8edd9-165">Podemos fazer isso adicionando um parâmetro "gênero" para nosso método de ação.</span><span class="sxs-lookup"><span data-stu-id="8edd9-165">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="8edd9-166">Quando fazemos isso ASP.NET MVC passará automaticamente qualquer parâmetro de postagem querystring ou formulário denominado "gênero" para nosso método de ação quando ele é invocado.</span><span class="sxs-lookup"><span data-stu-id="8edd9-166">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="8edd9-167">*Observação: Estamos usando o método de utilitário HttpUtility para limpar a entrada do usuário. Isso impede que os usuários de injeção de Javascript em nosso exibição com um link como /Store/Browse? Gênero =&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="8edd9-167">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="8edd9-168">Agora vamos procure/repositório/procurar? Gênero = Disco</span><span class="sxs-lookup"><span data-stu-id="8edd9-168">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="8edd9-169">Vamos Avançar alterar a ação de detalhes para ler e exibir um parâmetro de entrada chamado ID.</span><span class="sxs-lookup"><span data-stu-id="8edd9-169">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="8edd9-170">Ao contrário do nosso método anterior, nós não inserindo o valor da ID como um parâmetro querystring.</span><span class="sxs-lookup"><span data-stu-id="8edd9-170">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="8edd9-171">Em vez disso, podemos será incorporá-lo diretamente na URL de si mesmo.</span><span class="sxs-lookup"><span data-stu-id="8edd9-171">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="8edd9-172">Por exemplo: /Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="8edd9-172">For example: /Store/Details/5.</span></span>

<span data-ttu-id="8edd9-173">ASP.NET MVC permite fazer isso facilmente sem a necessidade de configurar nada.</span><span class="sxs-lookup"><span data-stu-id="8edd9-173">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="8edd9-174">Convenção de roteamento de ASP.NET MVC padrão é tratar o segmento de URL após o nome do método de ação como um parâmetro denominado "ID".</span><span class="sxs-lookup"><span data-stu-id="8edd9-174">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="8edd9-175">Se seu método de ação tem um parâmetro chamado ID, em seguida, o ASP.NET MVC automaticamente passará o segmento de URL para você como um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="8edd9-175">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="8edd9-176">Execute o aplicativo e navegue até /Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="8edd9-176">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="8edd9-177">Vamos recapitular o que fizemos até agora:</span><span class="sxs-lookup"><span data-stu-id="8edd9-177">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="8edd9-178">Criamos um novo projeto ASP.NET MVC no Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="8edd9-178">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="8edd9-179">Discutimos a estrutura de pasta básica de um aplicativo ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8edd9-179">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="8edd9-180">Aprendemos como executar nosso site usando o ASP.NET Development Server</span><span class="sxs-lookup"><span data-stu-id="8edd9-180">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="8edd9-181">Nós criamos duas classes do controlador: um HomeController e um StoreController</span><span class="sxs-lookup"><span data-stu-id="8edd9-181">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="8edd9-182">Adicionamos os métodos de ação ao nosso controladores que respondem às solicitações de URL e retornam o texto para o navegador</span><span class="sxs-lookup"><span data-stu-id="8edd9-182">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="8edd9-183">[Anterior](mvc-music-store-part-1.md)
> [Próximo](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="8edd9-183">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>

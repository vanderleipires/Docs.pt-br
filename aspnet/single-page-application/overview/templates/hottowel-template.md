---
uid: single-page-application/overview/templates/hottowel-template
title: Modelo de toalhas hot | Microsoft Docs
author: madskristensen
description: Modelo de HotTowel
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: dbd037c2469d326a3d3248ca07492ed9eb93e225
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="hot-towel-template"></a><span data-ttu-id="8f7de-103">Modelo de toalhas ativo</span><span class="sxs-lookup"><span data-stu-id="8f7de-103">Hot Towel template</span></span>
====================
<span data-ttu-id="8f7de-104">por [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="8f7de-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="8f7de-105">O modelo de MVC toalhas ativa é escrito por John Papa</span><span class="sxs-lookup"><span data-stu-id="8f7de-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="8f7de-106">Escolha a versão para download:</span><span class="sxs-lookup"><span data-stu-id="8f7de-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="8f7de-107">Modelo MVC toalhas ativa para o Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="8f7de-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="8f7de-108">Modelo MVC toalhas ativa para Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8f7de-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="8f7de-109">Hot toalhas: Porque você não deseja ir para o SPA sem um!</span><span class="sxs-lookup"><span data-stu-id="8f7de-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="8f7de-110">Deseja criar um SPA, mas não é possível decidir onde iniciar?</span><span class="sxs-lookup"><span data-stu-id="8f7de-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="8f7de-111">Usar um toalhas ativa e em segundos você terá um SPA e todas as ferramentas que necessárias para criar nele!</span><span class="sxs-lookup"><span data-stu-id="8f7de-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="8f7de-112">Toalhas hot cria um excelente ponto de partida para a criação de um aplicativo de página única (SPA) com o ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8f7de-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="8f7de-113">Sem a necessidade de você ele fornece uma estrutura modular para o código, modo de exibição de navegação, associação de dados, gerenciamento de dados avançado e estilo simple, mas elegante.</span><span class="sxs-lookup"><span data-stu-id="8f7de-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="8f7de-114">Toalhas hot fornece todo o que necessário para criar um SPA, para que você possa se concentrar em seu aplicativo, não o direcionamento.</span><span class="sxs-lookup"><span data-stu-id="8f7de-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="8f7de-115">Saiba mais sobre como criar um SPA de [John Papa vídeos, tutoriais e Pluralsight cursos](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="8f7de-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="8f7de-116">Estrutura de aplicativo</span><span class="sxs-lookup"><span data-stu-id="8f7de-116">Application Structure</span></span>

<span data-ttu-id="8f7de-117">Hot SPA de toalhas fornece uma pasta de aplicativo que contém os arquivos JavaScript e HTML que definem o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8f7de-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="8f7de-118">Dentro da pasta do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="8f7de-118">Inside the App folder:</span></span>

- <span data-ttu-id="8f7de-119">Durandal</span><span class="sxs-lookup"><span data-stu-id="8f7de-119">durandal</span></span>
- <span data-ttu-id="8f7de-120">serviços</span><span class="sxs-lookup"><span data-stu-id="8f7de-120">services</span></span>
- <span data-ttu-id="8f7de-121">ViewModels</span><span class="sxs-lookup"><span data-stu-id="8f7de-121">viewmodels</span></span>
- <span data-ttu-id="8f7de-122">modos de exibição</span><span class="sxs-lookup"><span data-stu-id="8f7de-122">views</span></span>

<span data-ttu-id="8f7de-123">A pasta de aplicativo contém uma coleção de módulos.</span><span class="sxs-lookup"><span data-stu-id="8f7de-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="8f7de-124">Esses módulos encapsulam funcionalidades e declarar dependências em outros módulos.</span><span class="sxs-lookup"><span data-stu-id="8f7de-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="8f7de-125">Modos de exibição contém o HTML para seu aplicativo e a pasta de viewmodels contém a lógica de apresentação para os modos de exibição (um padrão comum de MVVM).</span><span class="sxs-lookup"><span data-stu-id="8f7de-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="8f7de-126">A pasta de serviços é ideal para acomodar todos os serviços comuns que talvez seja necessário seu aplicativo, como recuperação de dados HTTP ou interação com o armazenamento local.</span><span class="sxs-lookup"><span data-stu-id="8f7de-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="8f7de-127">É comum para várias viewmodels reutilizar o código dos módulos de serviço.</span><span class="sxs-lookup"><span data-stu-id="8f7de-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="8f7de-128">Estrutura de aplicativo do ASP.NET MVC Server lado</span><span class="sxs-lookup"><span data-stu-id="8f7de-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="8f7de-129">Toalhas hot cria a estrutura ASP.NET MVC familiares e avançados.</span><span class="sxs-lookup"><span data-stu-id="8f7de-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="8f7de-130">Aplicativo\_iniciar</span><span class="sxs-lookup"><span data-stu-id="8f7de-130">App\_Start</span></span>
- <span data-ttu-id="8f7de-131">Conteúdo</span><span class="sxs-lookup"><span data-stu-id="8f7de-131">Content</span></span>
- <span data-ttu-id="8f7de-132">Controladores</span><span class="sxs-lookup"><span data-stu-id="8f7de-132">Controllers</span></span>
- <span data-ttu-id="8f7de-133">Modelos</span><span class="sxs-lookup"><span data-stu-id="8f7de-133">Models</span></span>
- <span data-ttu-id="8f7de-134">Scripts</span><span class="sxs-lookup"><span data-stu-id="8f7de-134">Scripts</span></span>
- <span data-ttu-id="8f7de-135">Exibições</span><span class="sxs-lookup"><span data-stu-id="8f7de-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="8f7de-136">Bibliotecas em destaque</span><span class="sxs-lookup"><span data-stu-id="8f7de-136">Featured Libraries</span></span>

- <span data-ttu-id="8f7de-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="8f7de-137">ASP.NET MVC</span></span>
- <span data-ttu-id="8f7de-138">API da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8f7de-138">ASP.NET Web API</span></span>
- <span data-ttu-id="8f7de-139">Otimização da Web do ASP.NET - empacotamento e minimização</span><span class="sxs-lookup"><span data-stu-id="8f7de-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="8f7de-140">[Breeze.js](http://Breezejs.com) -gerenciamento de dados avançados</span><span class="sxs-lookup"><span data-stu-id="8f7de-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="8f7de-141">[Durandal.js](http://Durandaljs.com) -navegação e a composição de exibição</span><span class="sxs-lookup"><span data-stu-id="8f7de-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="8f7de-142">[Knockout. js](http://Knockoutjs.com) -associações de dados</span><span class="sxs-lookup"><span data-stu-id="8f7de-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="8f7de-143">[Require.js](http://requirejs.org) -modularidade com AMD e de otimização</span><span class="sxs-lookup"><span data-stu-id="8f7de-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="8f7de-144">[Toastr.js](http://jpapa.me/c7toastr) -mensagens pop-up</span><span class="sxs-lookup"><span data-stu-id="8f7de-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="8f7de-145">[Inicialização do Twitter](http://twitter.github.com/bootstrap/) - robustas folhas de estilos</span><span class="sxs-lookup"><span data-stu-id="8f7de-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="8f7de-146">Instalando por meio do modelo do Visual Studio 2012 toalhas Hot SPA</span><span class="sxs-lookup"><span data-stu-id="8f7de-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="8f7de-147">Toalhas ativa podem ser instalada como um modelo do Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="8f7de-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="8f7de-148">Basta clicar em `File`  |  `New Project` e escolha `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="8f7de-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="8f7de-149">Selecione o ' aplicativo de página única toalhas Hot "modelo e execução!</span><span class="sxs-lookup"><span data-stu-id="8f7de-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="8f7de-150">Instalando por meio do pacote do NuGet</span><span class="sxs-lookup"><span data-stu-id="8f7de-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="8f7de-151">Toalhas hot também é um pacote do NuGet que aumenta um projeto ASP.NET MVC vazio existente.</span><span class="sxs-lookup"><span data-stu-id="8f7de-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="8f7de-152">Instalar apenas usando o Nuget e, em seguida, execute!</span><span class="sxs-lookup"><span data-stu-id="8f7de-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="8f7de-153">Como criar em um toalhas ativa?</span><span class="sxs-lookup"><span data-stu-id="8f7de-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="8f7de-154">Basta começar a adicionar código!</span><span class="sxs-lookup"><span data-stu-id="8f7de-154">Simply start adding code!</span></span>

1. <span data-ttu-id="8f7de-155">Adicionar seu próprio código do lado do servidor, preferencialmente o Entity Framework e WebAPI (o que realmente se destacam com Breeze.js)</span><span class="sxs-lookup"><span data-stu-id="8f7de-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="8f7de-156">Adicionar modos de exibição para o `App/views` pasta</span><span class="sxs-lookup"><span data-stu-id="8f7de-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="8f7de-157">Adicionar viewmodels para o `App/viewmodels` pasta</span><span class="sxs-lookup"><span data-stu-id="8f7de-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="8f7de-158">Adicionar associações de dados HTML e Knockout a novos modos de exibição</span><span class="sxs-lookup"><span data-stu-id="8f7de-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="8f7de-159">Atualizar as rotas de navegação `shell.js`</span><span class="sxs-lookup"><span data-stu-id="8f7de-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="8f7de-160">Instruções passo a passo do HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="8f7de-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="8f7de-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="8f7de-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="8f7de-162">cshtml é a rota inicial e o modo de exibição para o aplicativo MVC.</span><span class="sxs-lookup"><span data-stu-id="8f7de-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="8f7de-163">Ele contém todas as marcas meta padrão, links de css e você esperaria de referências de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="8f7de-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="8f7de-164">O corpo contém um único `<div>` que é onde todo o conteúdo (seus modos de exibição) será colocado quando forem solicitados.</span><span class="sxs-lookup"><span data-stu-id="8f7de-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="8f7de-165">O `@Scripts.Render` usa Require.js para executar o ponto de entrada para o código do aplicativo, que está contido no `main.js` arquivo.</span><span class="sxs-lookup"><span data-stu-id="8f7de-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="8f7de-166">Uma tela inicial é fornecida para demonstrar como criar uma tela inicial, enquanto seu aplicativo carrega.</span><span class="sxs-lookup"><span data-stu-id="8f7de-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="8f7de-167">App/Main.js</span><span class="sxs-lookup"><span data-stu-id="8f7de-167">App/main.js</span></span>

<span data-ttu-id="8f7de-168">O `main.js` arquivo contém o código que será executado assim que seu aplicativo é carregado.</span><span class="sxs-lookup"><span data-stu-id="8f7de-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="8f7de-169">Isso é onde você deseja definir seu rotas de navegação, defina seu início os modos de exibição e executar qualquer instalação/inicialização como desobstrução dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8f7de-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="8f7de-170">O `main.js` arquivo define vários dos módulos do durandal para iniciar o aplicativo Iniciar.</span><span class="sxs-lookup"><span data-stu-id="8f7de-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="8f7de-171">A instrução definir ajuda a resolver as dependências de módulos para que fiquem disponíveis para a função.</span><span class="sxs-lookup"><span data-stu-id="8f7de-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="8f7de-172">Primeiro as mensagens de depuração estão habilitadas, quais enviam mensagens sobre os eventos que está executando o aplicativo para a janela do console.</span><span class="sxs-lookup"><span data-stu-id="8f7de-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="8f7de-173">O código de App informa durandal framework para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8f7de-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="8f7de-174">As convenções são definidas para que durandal sabe todas as exibições e viewmodels contidos nas mesmas pastas nomeadas, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="8f7de-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="8f7de-175">Por fim, o `app.setRoot` inicia, carrega o `shell` usando um modelo predefinido `entrance` animação.</span><span class="sxs-lookup"><span data-stu-id="8f7de-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="8f7de-176">Exibições</span><span class="sxs-lookup"><span data-stu-id="8f7de-176">Views</span></span>

<span data-ttu-id="8f7de-177">Modos de exibição são encontrados no `App/views` pasta.</span><span class="sxs-lookup"><span data-stu-id="8f7de-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="8f7de-178">shell.html</span><span class="sxs-lookup"><span data-stu-id="8f7de-178">shell.html</span></span>

<span data-ttu-id="8f7de-179">O `shell.html` contém o layout mestre para HTML.</span><span class="sxs-lookup"><span data-stu-id="8f7de-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="8f7de-180">Todos os outros modos de exibição serão compostos em algum lugar no lado do seu `shell` exibição.</span><span class="sxs-lookup"><span data-stu-id="8f7de-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="8f7de-181">Toalhas hot fornece um `shell` com três regiões: um cabeçalho, uma área de conteúdo e um rodapé.</span><span class="sxs-lookup"><span data-stu-id="8f7de-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="8f7de-182">Cada uma dessas regiões é carregada com conteúdo formam a outros modos de exibição quando solicitado.</span><span class="sxs-lookup"><span data-stu-id="8f7de-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="8f7de-183">O `compose` associações para o cabeçalho e rodapé são codificados em um toalhas ativa para apontar para o `nav` e `footer` exibições, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="8f7de-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="8f7de-184">A associação de redação para a seção `#content` está associada ao `router` item ativo do módulo.</span><span class="sxs-lookup"><span data-stu-id="8f7de-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="8f7de-185">Em outras palavras, quando você clica em um link de navegação é o modo de exibição correspondente é carregado nessa área.</span><span class="sxs-lookup"><span data-stu-id="8f7de-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="8f7de-186">nav.html</span><span class="sxs-lookup"><span data-stu-id="8f7de-186">nav.html</span></span>

<span data-ttu-id="8f7de-187">O `nav.html` contém os links de navegação para o SPA.</span><span class="sxs-lookup"><span data-stu-id="8f7de-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="8f7de-188">Isso é onde a estrutura de menu pode ser colocada, por exemplo.</span><span class="sxs-lookup"><span data-stu-id="8f7de-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="8f7de-189">Geralmente é associados (usando Knockout) de dados para o `router` módulo para exibir a navegação definido no `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="8f7de-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="8f7de-190">Knockout procura a associação de dados de atributos e associa-os para o `shell` viewmodel para exibir as rotas de navegação e mostrar uma progressbar (usando a inicialização do Twitter) se o `router` módulo está no meio de navegação de um modo de exibição para outra (consulte `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="8f7de-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="8f7de-191">o arquivo Home.HTML e details.html</span><span class="sxs-lookup"><span data-stu-id="8f7de-191">home.html and details.html</span></span>

<span data-ttu-id="8f7de-192">Essas exibições contêm HTML para exibições personalizadas.</span><span class="sxs-lookup"><span data-stu-id="8f7de-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="8f7de-193">Quando o `home` link no `nav` menu do modo de exibição é clicado, o `home` exibição será colocada na área de conteúdo do `shell` exibição.</span><span class="sxs-lookup"><span data-stu-id="8f7de-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="8f7de-194">Esses modos de exibição podem ser aumentados ou substituídos por suas próprias exibições personalizadas.</span><span class="sxs-lookup"><span data-stu-id="8f7de-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="8f7de-195">footer.html</span><span class="sxs-lookup"><span data-stu-id="8f7de-195">footer.html</span></span>

<span data-ttu-id="8f7de-196">O `footer.html` contém HTML que aparece no rodapé, na parte inferior do `shell` exibição.</span><span class="sxs-lookup"><span data-stu-id="8f7de-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="8f7de-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="8f7de-197">ViewModels</span></span>

<span data-ttu-id="8f7de-198">ViewModels são encontrados no `App/viewmodels` pasta.</span><span class="sxs-lookup"><span data-stu-id="8f7de-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="8f7de-199">Shell.js</span><span class="sxs-lookup"><span data-stu-id="8f7de-199">shell.js</span></span>

<span data-ttu-id="8f7de-200">O `shell` viewmodel contém as propriedades e funções que estão associadas ao `shell` exibição.</span><span class="sxs-lookup"><span data-stu-id="8f7de-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="8f7de-201">Geralmente, isso é onde as associações de navegação do menu são encontradas (consulte o `router.mapNav` lógica).</span><span class="sxs-lookup"><span data-stu-id="8f7de-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="8f7de-202">Home.js e details.js</span><span class="sxs-lookup"><span data-stu-id="8f7de-202">home.js and details.js</span></span>

<span data-ttu-id="8f7de-203">Esses viewmodels contêm as propriedades e as funções que estão associadas ao `home` exibição.</span><span class="sxs-lookup"><span data-stu-id="8f7de-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="8f7de-204">Ele também contém a lógica de apresentação para o modo de exibição e é a união entre os dados e o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="8f7de-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="8f7de-205">Serviços</span><span class="sxs-lookup"><span data-stu-id="8f7de-205">Services</span></span>

<span data-ttu-id="8f7de-206">Serviços são localizados na pasta/serviços de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="8f7de-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="8f7de-207">Idealmente, os serviços futuros como um módulo dataservice, que é responsável por obter e postar dados remotos, podem ser colocados.</span><span class="sxs-lookup"><span data-stu-id="8f7de-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="8f7de-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="8f7de-208">logger.js</span></span>

<span data-ttu-id="8f7de-209">Toalhas hot fornece um `logger` módulo na pasta serviços.</span><span class="sxs-lookup"><span data-stu-id="8f7de-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="8f7de-210">O `logger` módulo é ideal para as mensagens de log para o console e o usuário no pop-up notificações do sistema.</span><span class="sxs-lookup"><span data-stu-id="8f7de-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>

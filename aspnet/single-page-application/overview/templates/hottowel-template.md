---
uid: single-page-application/overview/templates/hottowel-template
title: Modelo de hot Towel | Microsoft Docs
author: madskristensen
description: Modelo de HotTowel
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: de81f12f57d7f2fb7c6478bfa1f3a278ae905a39
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388527"
---
<a name="hot-towel-template"></a><span data-ttu-id="fa6c9-103">Modelo hot Towel</span><span class="sxs-lookup"><span data-stu-id="fa6c9-103">Hot Towel template</span></span>
====================
<span data-ttu-id="fa6c9-104">por [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="fa6c9-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="fa6c9-105">O modelo de MVC Hot Towel é escrito por John Papa</span><span class="sxs-lookup"><span data-stu-id="fa6c9-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="fa6c9-106">Escolha a versão para baixar:</span><span class="sxs-lookup"><span data-stu-id="fa6c9-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="fa6c9-107">Modelo do MVC hot Towel para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="fa6c9-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="fa6c9-108">Modelo do MVC hot Towel para Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="fa6c9-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="fa6c9-109">Hot Towel: Porque você não deseja ir para o SPA sem um!</span><span class="sxs-lookup"><span data-stu-id="fa6c9-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="fa6c9-110">Deseja criar um SPA, mas não souber por onde começar?</span><span class="sxs-lookup"><span data-stu-id="fa6c9-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="fa6c9-111">Use o Hot Towel e em segundos você terá um SPA e todas as ferramentas que você precisa para compilar nele!</span><span class="sxs-lookup"><span data-stu-id="fa6c9-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="fa6c9-112">Hot Towel cria um ótimo ponto de partida para a criação de um aplicativo de página única (SPA) com o ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="fa6c9-113">Fora da caixa você ele fornece uma estrutura modular para seu código, navegação de modo de exibição, vinculação de dados, gerenciamento de dados avançados e estilo simple, porém elegante.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="fa6c9-114">Hot Towel fornece tudo o que você precisa para criar um SPA, portanto, você pode se concentrar em seu aplicativo, não os detalhes técnicos.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="fa6c9-115">Saiba mais sobre como criar um SPA partir [John Papa vídeos, tutoriais e cursos da Pluralsight](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="fa6c9-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="fa6c9-116">Estrutura de aplicativo</span><span class="sxs-lookup"><span data-stu-id="fa6c9-116">Application Structure</span></span>

<span data-ttu-id="fa6c9-117">SPA de hot Towel fornece uma pasta do aplicativo que contém os arquivos JavaScript e HTML que definem o seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="fa6c9-118">Dentro da pasta de aplicativo:</span><span class="sxs-lookup"><span data-stu-id="fa6c9-118">Inside the App folder:</span></span>

- <span data-ttu-id="fa6c9-119">durandal</span><span class="sxs-lookup"><span data-stu-id="fa6c9-119">durandal</span></span>
- <span data-ttu-id="fa6c9-120">serviços</span><span class="sxs-lookup"><span data-stu-id="fa6c9-120">services</span></span>
- <span data-ttu-id="fa6c9-121">ViewModels</span><span class="sxs-lookup"><span data-stu-id="fa6c9-121">viewmodels</span></span>
- <span data-ttu-id="fa6c9-122">modos de exibição</span><span class="sxs-lookup"><span data-stu-id="fa6c9-122">views</span></span>

<span data-ttu-id="fa6c9-123">A pasta de aplicativo contém uma coleção de módulos.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="fa6c9-124">Esses módulos encapsulam a funcionalidade e declarar dependências nos outros módulos.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="fa6c9-125">A pasta de modos de exibição contém o HTML para seu aplicativo e a pasta viewmodels contém a lógica de apresentação para os modos de exibição (um padrão MVVM comum).</span><span class="sxs-lookup"><span data-stu-id="fa6c9-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="fa6c9-126">A pasta de serviços é ideal para hospedar todos os serviços comuns que seu aplicativo pode precisar, como recuperação de dados HTTP ou interação com o armazenamento local.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="fa6c9-127">É comum para vários viewmodels usar novamente o código dos módulos de serviço.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="fa6c9-128">Estrutura de aplicativo do lado de servidor de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="fa6c9-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="fa6c9-129">Hot Towel baseia-se a estrutura MVC do ASP.NET familiar e avançada.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="fa6c9-130">Aplicativo\_iniciar</span><span class="sxs-lookup"><span data-stu-id="fa6c9-130">App\_Start</span></span>
- <span data-ttu-id="fa6c9-131">Conteúdo</span><span class="sxs-lookup"><span data-stu-id="fa6c9-131">Content</span></span>
- <span data-ttu-id="fa6c9-132">Controladores</span><span class="sxs-lookup"><span data-stu-id="fa6c9-132">Controllers</span></span>
- <span data-ttu-id="fa6c9-133">Modelos</span><span class="sxs-lookup"><span data-stu-id="fa6c9-133">Models</span></span>
- <span data-ttu-id="fa6c9-134">Scripts</span><span class="sxs-lookup"><span data-stu-id="fa6c9-134">Scripts</span></span>
- <span data-ttu-id="fa6c9-135">Exibições</span><span class="sxs-lookup"><span data-stu-id="fa6c9-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="fa6c9-136">Bibliotecas em destaque</span><span class="sxs-lookup"><span data-stu-id="fa6c9-136">Featured Libraries</span></span>

- <span data-ttu-id="fa6c9-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="fa6c9-137">ASP.NET MVC</span></span>
- <span data-ttu-id="fa6c9-138">API da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="fa6c9-138">ASP.NET Web API</span></span>
- <span data-ttu-id="fa6c9-139">Otimização da Web do ASP.NET - agrupamento e minificação</span><span class="sxs-lookup"><span data-stu-id="fa6c9-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="fa6c9-140">[Breeze](http://Breezejs.com) -gerenciamento de dados avançados</span><span class="sxs-lookup"><span data-stu-id="fa6c9-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="fa6c9-141">[Durandal](http://Durandaljs.com) -navegação e a exibição de composição</span><span class="sxs-lookup"><span data-stu-id="fa6c9-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="fa6c9-142">[Knockout. js](http://Knockoutjs.com) -associações de dados</span><span class="sxs-lookup"><span data-stu-id="fa6c9-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="fa6c9-143">[Require](http://requirejs.org) -modularidade com AMD e otimização</span><span class="sxs-lookup"><span data-stu-id="fa6c9-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="fa6c9-144">[Toastr.js](http://jpapa.me/c7toastr) -mensagens pop-up</span><span class="sxs-lookup"><span data-stu-id="fa6c9-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="fa6c9-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - estilo de CSS robusta</span><span class="sxs-lookup"><span data-stu-id="fa6c9-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="fa6c9-146">Instalando por meio do modelo SPA do Visual Studio 2012 Hot Towel</span><span class="sxs-lookup"><span data-stu-id="fa6c9-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="fa6c9-147">Hot Towel pode ser instalado como um modelo do Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="fa6c9-148">Basta clicar `File`  |  `New Project` e escolha `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="fa6c9-149">Em seguida, selecione o ' Hot Towel aplicativos de uma página "modelo e execução!</span><span class="sxs-lookup"><span data-stu-id="fa6c9-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="fa6c9-150">Instalando por meio do pacote NuGet</span><span class="sxs-lookup"><span data-stu-id="fa6c9-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="fa6c9-151">Hot Towel também é um pacote do NuGet que incrementa um projeto ASP.NET MVC vazio existente.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="fa6c9-152">Instalar apenas usando o Nuget e, em seguida, executar!</span><span class="sxs-lookup"><span data-stu-id="fa6c9-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="fa6c9-153">Como criar no Hot Towel?</span><span class="sxs-lookup"><span data-stu-id="fa6c9-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="fa6c9-154">Basta começar a adicionar código!</span><span class="sxs-lookup"><span data-stu-id="fa6c9-154">Simply start adding code!</span></span>

1. <span data-ttu-id="fa6c9-155">Adicionar seu próprio código do lado do servidor, preferencialmente o Entity Framework e API da Web (o que realmente se destacam com a Breeze)</span><span class="sxs-lookup"><span data-stu-id="fa6c9-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="fa6c9-156">Adicionar modos de exibição para o `App/views` pasta</span><span class="sxs-lookup"><span data-stu-id="fa6c9-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="fa6c9-157">Adicionar viewmodels para o `App/viewmodels` pasta</span><span class="sxs-lookup"><span data-stu-id="fa6c9-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="fa6c9-158">Adicionar associações de dados HTML e o Knockout novos modos de exibição</span><span class="sxs-lookup"><span data-stu-id="fa6c9-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="fa6c9-159">Atualizar as rotas de navegação em `shell.js`</span><span class="sxs-lookup"><span data-stu-id="fa6c9-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="fa6c9-160">Instruções passo a passo do HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="fa6c9-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="fa6c9-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="fa6c9-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="fa6c9-162">index. cshtml é a rota inicial e o modo de exibição para o aplicativo MVC.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="fa6c9-163">Ele contém todas as metamarcas padrão, links de css e referências de JavaScript que você esperaria.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="fa6c9-164">O corpo contém um único `<div>` que é onde todo o conteúdo (seus modos de exibição) será colocado quando eles são solicitados.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="fa6c9-165">O `@Scripts.Render` usa Require para executar o ponto de entrada para o código do aplicativo, que está contido no `main.js` arquivo.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="fa6c9-166">Uma tela inicial é fornecida para demonstrar como criar uma tela inicial, enquanto o aplicativo é carregado.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="fa6c9-167">App/Main.js</span><span class="sxs-lookup"><span data-stu-id="fa6c9-167">App/main.js</span></span>

<span data-ttu-id="fa6c9-168">O `main.js` arquivo contém o código que será executado assim que seu aplicativo é carregado.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="fa6c9-169">Isso é onde você deseja define suas rotas de navegação, defina seu início os modos de exibição e executar qualquer programa de instalação/inicialização como uma desobstrução os dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="fa6c9-170">O `main.js` arquivo define vários dos módulos do durandal para ajudar a iniciar o aplicativo Iniciar.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="fa6c9-171">A instrução definir ajuda a resolver as dependências de módulos para que fiquem disponíveis para a função.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="fa6c9-172">Primeiro as mensagens de depuração estiverem habilitadas, que enviam mensagens sobre quais eventos que o aplicativo está executando a janela do console.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="fa6c9-173">O código App.Start.&lt;2} informa a estrutura de durandal para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="fa6c9-174">As convenções são definidas para que o durandal sabe todas as exibições e viewmodels estão contidos nas mesmas pastas nomeadas, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="fa6c9-175">Por fim, o `app.setRoot` é acionada carrega o `shell` usando um modelo predefinido `entrance` animação.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="fa6c9-176">Exibições</span><span class="sxs-lookup"><span data-stu-id="fa6c9-176">Views</span></span>

<span data-ttu-id="fa6c9-177">Modos de exibição são encontrados no `App/views` pasta.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="fa6c9-178">shell.html</span><span class="sxs-lookup"><span data-stu-id="fa6c9-178">shell.html</span></span>

<span data-ttu-id="fa6c9-179">O `shell.html` contém o layout mestre para seu HTML.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="fa6c9-180">Todos os outros modos de exibição serão compostos em algum lugar no lado do seu `shell` modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="fa6c9-181">Hot Towel fornece um `shell` com três regiões: um cabeçalho, uma área de conteúdo e um rodapé.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="fa6c9-182">Cada uma dessas regiões é carregada com conteúdo forma a outros modos de exibição quando solicitado.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="fa6c9-183">O `compose` associações para o cabeçalho e rodapé são codificados em Hot Towel para apontar para o `nav` e `footer` exibe, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="fa6c9-184">A associação de composição para a seção `#content` está associado ao `router` o item do Active Directory do módulo.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="fa6c9-185">Em outras palavras, quando você clica em um link de navegação é o modo de exibição correspondente é carregado nessa área.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="fa6c9-186">nav.html</span><span class="sxs-lookup"><span data-stu-id="fa6c9-186">nav.html</span></span>

<span data-ttu-id="fa6c9-187">O `nav.html` contém os links de navegação para o SPA.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="fa6c9-188">Isso é onde a estrutura do menu pode ser colocada, por exemplo.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="fa6c9-189">Geralmente isso é dados vinculados (usando o Knockout) para o `router` módulo para exibir a navegação que você definiu no `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="fa6c9-190">O Knockout procura a vinculação de dados, atributos e associa-os para o `shell` viewmodel para exibir as rotas de navegação e mostrar uma barra de progresso (usando o Twitter Bootstrap) se o `router` módulo está no meio de navegação de um modo de exibição para outra (consulte `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="fa6c9-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="fa6c9-191">Home. HTML e details.html</span><span class="sxs-lookup"><span data-stu-id="fa6c9-191">home.html and details.html</span></span>

<span data-ttu-id="fa6c9-192">Essas exibições contêm HTML para exibições personalizadas.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="fa6c9-193">Quando o `home` link na `nav` menu do modo de exibição é clicado, o `home` modo de exibição será colocado na área de conteúdo do `shell` modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="fa6c9-194">Esses modos de exibição podem ser aumentados ou substituídos por suas próprias exibições personalizadas.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="fa6c9-195">footer.html</span><span class="sxs-lookup"><span data-stu-id="fa6c9-195">footer.html</span></span>

<span data-ttu-id="fa6c9-196">O `footer.html` contém o HTML que aparece no rodapé, na parte inferior do `shell` modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="fa6c9-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="fa6c9-197">ViewModels</span></span>

<span data-ttu-id="fa6c9-198">ViewModels são encontrados no `App/viewmodels` pasta.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="fa6c9-199">Shell.js</span><span class="sxs-lookup"><span data-stu-id="fa6c9-199">shell.js</span></span>

<span data-ttu-id="fa6c9-200">O `shell` viewmodel contém propriedades e funções que estão associadas ao `shell` modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="fa6c9-201">Geralmente, isso é onde as associações de navegação de menu são encontradas (consulte a `router.mapNav` lógica).</span><span class="sxs-lookup"><span data-stu-id="fa6c9-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="fa6c9-202">Home. js e details.js</span><span class="sxs-lookup"><span data-stu-id="fa6c9-202">home.js and details.js</span></span>

<span data-ttu-id="fa6c9-203">Esses viewmodels contêm as propriedades e funções que estão associadas ao `home` modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="fa6c9-204">Ele também contém a lógica de apresentação para o modo de exibição e é a cola entre os dados e o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="fa6c9-205">Serviços</span><span class="sxs-lookup"><span data-stu-id="fa6c9-205">Services</span></span>

<span data-ttu-id="fa6c9-206">Serviços são localizados na pasta/serviços de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="fa6c9-207">O ideal é que os serviços futuros, como um módulo dataservice, que é responsável por obter e postar dados remotos, poderia ser colocados.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="fa6c9-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="fa6c9-208">logger.js</span></span>

<span data-ttu-id="fa6c9-209">Hot Towel fornece um `logger` módulo na pasta de serviços.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="fa6c9-210">O `logger` módulo é ideal para as mensagens de log para o console e ao usuário no pop-up de notificações do sistema.</span><span class="sxs-lookup"><span data-stu-id="fa6c9-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>

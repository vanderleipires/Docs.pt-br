---
title: "Usando AngularJS para aplicativos de página única (SPAs)"
author: rick-anderson
description: Saiba como criar um aplicativo ASP.NET SPA estilo usando AngularJS
keywords: "Núcleo do ASP.NET, AngularJS, SPA"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/angular
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ccdf1625cdaf2400780500ac5ab86f41537964a9
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a><span data-ttu-id="2dffa-104">Usando AngularJS para aplicativos de página única (SPAs) com o ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2dffa-104">Using AngularJS for Single Page Applications (SPAs) with ASP.NET Core</span></span>


<span data-ttu-id="2dffa-105">Por [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="2dffa-105">By [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="2dffa-106">Neste artigo, você aprenderá como criar um aplicativo ASP.NET SPA estilo usando AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2dffa-106">In this article, you will learn how to build a SPA-style ASP.NET application using AngularJS.</span></span>

<span data-ttu-id="2dffa-107">[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2dffa-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-angularjs"></a><span data-ttu-id="2dffa-108">O que é AngularJS?</span><span class="sxs-lookup"><span data-stu-id="2dffa-108">What is AngularJS?</span></span>

<span data-ttu-id="2dffa-109">[AngularJS](https://angularjs.org/) é uma estrutura moderna de JavaScript do Google comumente usado para trabalhar com aplicativos de página única (SPAs).</span><span class="sxs-lookup"><span data-stu-id="2dffa-109">[AngularJS](https://angularjs.org/) is a modern JavaScript framework from Google commonly used to work with Single Page Applications (SPAs).</span></span> <span data-ttu-id="2dffa-110">AngularJS aberta originado sob a licença do MIT, e o progresso de desenvolvimento de AngularJS pode ser seguido [seu repositório GitHub](https://github.com/angular/angular.js).</span><span class="sxs-lookup"><span data-stu-id="2dffa-110">AngularJS is open sourced under MIT license, and the development progress of AngularJS can be followed on [its GitHub repository](https://github.com/angular/angular.js).</span></span> <span data-ttu-id="2dffa-111">A biblioteca é chamada Angular como HTML usa colchetes angulares formatada.</span><span class="sxs-lookup"><span data-stu-id="2dffa-111">The library is called Angular because HTML uses angular-shaped brackets.</span></span>

<span data-ttu-id="2dffa-112">AngularJS não é uma biblioteca de manipulação de DOM como jQuery, mas ele usa um subconjunto de jQuery chamado jQLite.</span><span class="sxs-lookup"><span data-stu-id="2dffa-112">AngularJS is not a DOM manipulation library like jQuery, but it uses a subset of jQuery called jQLite.</span></span> <span data-ttu-id="2dffa-113">AngularJS baseia-se principalmente a declarativos atributos HTML que você pode adicionar a suas marcas HTML.</span><span class="sxs-lookup"><span data-stu-id="2dffa-113">AngularJS is primarily based on declarative HTML attributes that you can add to your HTML tags.</span></span> <span data-ttu-id="2dffa-114">Você pode tentar AngularJS em seu navegador usando o [site código escola](https://www.codeschool.com/courses/shaping-up-with-angularjs) ou [W3Schools site](https://www.w3schools.com/angular/).</span><span class="sxs-lookup"><span data-stu-id="2dffa-114">You can try AngularJS in your browser using the [Code School website](https://www.codeschool.com/courses/shaping-up-with-angularjs) or  [W3Schools website](https://www.w3schools.com/angular/).</span></span>

<span data-ttu-id="2dffa-115">Este artigo se concentra em AngularJS com algumas observações sobre onde Angular é título.</span><span class="sxs-lookup"><span data-stu-id="2dffa-115">This article focuses on AngularJS with some notes on where Angular is heading.</span></span>

## <a name="getting-started"></a><span data-ttu-id="2dffa-116">Introdução</span><span class="sxs-lookup"><span data-stu-id="2dffa-116">Getting started</span></span>

<span data-ttu-id="2dffa-117">Para começar a usar o AngularJS em seu aplicativo ASP.NET, você deve instalá-lo como parte de seu projeto ou referenciá-lo a partir de uma rede de fornecimento de conteúdo (CDN).</span><span class="sxs-lookup"><span data-stu-id="2dffa-117">To start using AngularJS in your ASP.NET application, you must either install it as part of your project, or reference it from a content delivery network (CDN).</span></span>

### <a name="installation"></a><span data-ttu-id="2dffa-118">Instalação</span><span class="sxs-lookup"><span data-stu-id="2dffa-118">Installation</span></span>

<span data-ttu-id="2dffa-119">Há várias maneiras de adicionar AngularJS ao seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2dffa-119">There are several ways to add AngularJS to your application.</span></span> <span data-ttu-id="2dffa-120">Se você estiver iniciando um novo aplicativo web ASP.NET Core no Visual Studio, você pode adicionar AngularJS usando o interno [Bower](bower.md) suporte.</span><span class="sxs-lookup"><span data-stu-id="2dffa-120">If you’re starting a new ASP.NET Core web application in Visual Studio, you can add AngularJS using the built-in [Bower](bower.md) support.</span></span> <span data-ttu-id="2dffa-121">Abra *bower. JSON*e adicione uma entrada para o `dependencies` propriedade:</span><span class="sxs-lookup"><span data-stu-id="2dffa-121">Open *bower.json*, and add an entry to the `dependencies` property:</span></span>

<a name="angular-bower-json"></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

<span data-ttu-id="2dffa-122">Ao salvar o *bower. JSON* , Angular será instalado em seu projeto *wwwroot/lib* pasta.</span><span class="sxs-lookup"><span data-stu-id="2dffa-122">Upon saving the *bower.json* file, Angular will be installed in your project's *wwwroot/lib* folder.</span></span> <span data-ttu-id="2dffa-123">Além disso, ele será listado dentro do `Dependencies/Bower` pasta.</span><span class="sxs-lookup"><span data-stu-id="2dffa-123">Additionally, it will be listed within the `Dependencies/Bower` folder.</span></span> <span data-ttu-id="2dffa-124">Consulte a captura de tela abaixo.</span><span class="sxs-lookup"><span data-stu-id="2dffa-124">See the screenshot below.</span></span>

![Gerenciador de soluções com o Project AngularJS](angular/_static/angular-solution-explorer.png)

<span data-ttu-id="2dffa-126">Em seguida, adicione um `<script>` referência à parte inferior do `<body>` seção da página HTML ou *cshtml* de arquivo, como mostrado aqui:</span><span class="sxs-lookup"><span data-stu-id="2dffa-126">Next, add a `<script>` reference to the bottom of the `<body>` section of your HTML page or *_Layout.cshtml* file, as shown here:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

<span data-ttu-id="2dffa-127">É recomendável que os aplicativos de produção utilizam CDNs para bibliotecas comuns como AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2dffa-127">It's recommended that production applications utilize CDNs for common libraries like AngularJS.</span></span> <span data-ttu-id="2dffa-128">Você pode fazer referência a AngularJS de uma das várias CDNs, como este:</span><span class="sxs-lookup"><span data-stu-id="2dffa-128">You can reference AngularJS from one of several CDNs, such as this one:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

<span data-ttu-id="2dffa-129">Quando você tem uma referência para o *angular.js* arquivo de script, você está pronto para começar a usar o AngularJS nas páginas da web.</span><span class="sxs-lookup"><span data-stu-id="2dffa-129">Once you have a reference to the *angular.js* script file, you're ready to begin using AngularJS in your web pages.</span></span>

## <a name="key-components"></a><span data-ttu-id="2dffa-130">Componentes principais</span><span class="sxs-lookup"><span data-stu-id="2dffa-130">Key components</span></span>

<span data-ttu-id="2dffa-131">AngularJS inclui um número de componentes principais, como *diretivas*, *modelos*, *repetidores*, *módulos*,  *controladores*, *componentes*, *roteador componente* e muito mais.</span><span class="sxs-lookup"><span data-stu-id="2dffa-131">AngularJS includes a number of major components, such as *directives*, *templates*, *repeaters*, *modules*, *controllers*, *components*, *component router* and more.</span></span> <span data-ttu-id="2dffa-132">Vamos examinar como esses componentes trabalham juntos para adicionar um comportamento a páginas da web.</span><span class="sxs-lookup"><span data-stu-id="2dffa-132">Let's examine how these components work together to add behavior to your web pages.</span></span>

### <a name="directives"></a><span data-ttu-id="2dffa-133">Diretivas</span><span class="sxs-lookup"><span data-stu-id="2dffa-133">Directives</span></span>

<span data-ttu-id="2dffa-134">Usa AngularJS [diretivas](https://docs.angularjs.org/guide/directive) para estender o HTML com os elementos e atributos personalizados.</span><span class="sxs-lookup"><span data-stu-id="2dffa-134">AngularJS uses [directives](https://docs.angularjs.org/guide/directive) to extend HTML with custom attributes and elements.</span></span> <span data-ttu-id="2dffa-135">Diretivas de AngularJS são definidas por meio de `data-ng-*` ou `ng-*` prefixos (`ng` é a abreviação de angular).</span><span class="sxs-lookup"><span data-stu-id="2dffa-135">AngularJS directives are defined via `data-ng-*` or `ng-*` prefixes (`ng` is short for angular).</span></span> <span data-ttu-id="2dffa-136">Há dois tipos de diretivas do AngularJS:</span><span class="sxs-lookup"><span data-stu-id="2dffa-136">There are two types of AngularJS directives:</span></span>

   1. <span data-ttu-id="2dffa-137">**Diretivas primitivo**: esses são predefinidos pela equipe de Angular e fazem parte do framework AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2dffa-137">**Primitive Directives**: These are predefined by the Angular team and are part of the AngularJS framework.</span></span>

   2. <span data-ttu-id="2dffa-138">**Diretivas personalizadas**: esses são diretivas personalizadas que você pode definir.</span><span class="sxs-lookup"><span data-stu-id="2dffa-138">**Custom Directives**: These are custom directives that you can define.</span></span>

<span data-ttu-id="2dffa-139">Uma das diretivas primitivo usadas em todos os aplicativos de AngularJS é o `ng-app` diretiva, que inicializa o aplicativo AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2dffa-139">One of the primitive directives used in all AngularJS applications is the `ng-app` directive, which bootstraps the AngularJS application.</span></span> <span data-ttu-id="2dffa-140">Essa diretiva pode ser aplicada para o `<body>` marca ou a um elemento filho do corpo.</span><span class="sxs-lookup"><span data-stu-id="2dffa-140">This directive can be applied to the `<body>` tag or to a child element of the body.</span></span> <span data-ttu-id="2dffa-141">Vejamos um exemplo em ação.</span><span class="sxs-lookup"><span data-stu-id="2dffa-141">Let's see an example in action.</span></span> <span data-ttu-id="2dffa-142">Supondo que você está em um projeto do ASP.NET, você pode adicionar um arquivo HTML para o `wwwroot` pasta, ou adicionar uma nova ação de controlador e uma exibição associada.</span><span class="sxs-lookup"><span data-stu-id="2dffa-142">Assuming you're in an ASP.NET project, you can either add an HTML file to the `wwwroot` folder, or add a new controller action and an associated view.</span></span> <span data-ttu-id="2dffa-143">Nesse caso, adicionei um novo `Directives` método de ação `HomeController.cs`.</span><span class="sxs-lookup"><span data-stu-id="2dffa-143">In this case, I've added a new `Directives` action method to `HomeController.cs`.</span></span> <span data-ttu-id="2dffa-144">A exibição associada é mostrada aqui:</span><span class="sxs-lookup"><span data-stu-id="2dffa-144">The associated view is shown here:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

<span data-ttu-id="2dffa-145">Para manter esses exemplos independentes um do outro, não estou usando o arquivo de layout compartilhada.</span><span class="sxs-lookup"><span data-stu-id="2dffa-145">To keep these samples independent of one another, I'm not using the shared layout file.</span></span> <span data-ttu-id="2dffa-146">Você pode ver que estamos decorado marca body com o `ng-app` diretiva para indicar esta página é um aplicativo AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2dffa-146">You can see that we decorated the body tag with the `ng-app` directive to indicate this page is an AngularJS application.</span></span> <span data-ttu-id="2dffa-147">O `{{2+2}}` é uma expressão de associação de dados Angular que você aprenderá mais sobre daqui a pouco.</span><span class="sxs-lookup"><span data-stu-id="2dffa-147">The `{{2+2}}` is an Angular data binding expression that you will learn more about in a moment.</span></span> <span data-ttu-id="2dffa-148">Aqui está o resultado se você executar este aplicativo:</span><span class="sxs-lookup"><span data-stu-id="2dffa-148">Here is the result if you run this application:</span></span>

![Diretiva Angular Simple](angular/_static/simple-directive.png)

<span data-ttu-id="2dffa-150">Outros primitivas diretivas em AngularJS incluem:</span><span class="sxs-lookup"><span data-stu-id="2dffa-150">Other primitive directives in AngularJS include:</span></span>

<span data-ttu-id="2dffa-151">`ng-controller`Determina qual controlador de JavaScript está associado ao modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="2dffa-151">`ng-controller` Determines which JavaScript controller is bound to which view.</span></span>

<span data-ttu-id="2dffa-152">`ng-model`Determina o modelo ao qual os valores das propriedades de um elemento HTML estão associados.</span><span class="sxs-lookup"><span data-stu-id="2dffa-152">`ng-model` Determines the model to which the values of an HTML element's properties are bound.</span></span>

<span data-ttu-id="2dffa-153">`ng-init`Usado para inicializar os dados de aplicativo na forma de uma expressão para o escopo atual.</span><span class="sxs-lookup"><span data-stu-id="2dffa-153">`ng-init` Used to initialize the application data in the form of an expression for the current scope.</span></span>

<span data-ttu-id="2dffa-154">`ng-if`Remove ou recria o determinado elemento HTML no DOM com base em truthiness da expressão fornecida.</span><span class="sxs-lookup"><span data-stu-id="2dffa-154">`ng-if` Removes or recreates the given HTML element in the DOM based on the truthiness of the expression provided.</span></span>

<span data-ttu-id="2dffa-155">`ng-repeat`Repete um determinado bloco de HTML em um conjunto de dados.</span><span class="sxs-lookup"><span data-stu-id="2dffa-155">`ng-repeat` Repeats a given block of HTML over a set of data.</span></span>

<span data-ttu-id="2dffa-156">`ng-show`Mostra ou oculta o elemento HTML determinado de acordo com a expressão fornecida.</span><span class="sxs-lookup"><span data-stu-id="2dffa-156">`ng-show` Shows or hides the given HTML element based on the expression provided.</span></span>

<span data-ttu-id="2dffa-157">Para obter uma lista completa de todas as diretivas primitivo com suporte no AngularJS, consulte o [seção de diretiva de documentação no site de documentação do AngularJS](https://docs.angularjs.org/api/ng/directive).</span><span class="sxs-lookup"><span data-stu-id="2dffa-157">For a full list of all primitive directives supported in AngularJS, please refer to the [directive documentation section on the AngularJS documentation website](https://docs.angularjs.org/api/ng/directive).</span></span>

### <a name="data-binding"></a><span data-ttu-id="2dffa-158">Associação de dados</span><span class="sxs-lookup"><span data-stu-id="2dffa-158">Data binding</span></span>

<span data-ttu-id="2dffa-159">Fornece AngularJS [associação de dados](https://docs.angularjs.org/guide/databinding) suporte de-prontos usando o `ng-bind` diretiva ou uma sintaxe de expressão de associação, como de dados `{{expression}}`.</span><span class="sxs-lookup"><span data-stu-id="2dffa-159">AngularJS provides [data binding](https://docs.angularjs.org/guide/databinding) support out-of-the-box using either the `ng-bind` directive or a data binding expression syntax such as `{{expression}}`.</span></span> <span data-ttu-id="2dffa-160">AngularJS dá suporte à associação de dados bidirecional onde os dados de um modelo são mantidos em sincronização com um modelo de exibição em todos os momentos.</span><span class="sxs-lookup"><span data-stu-id="2dffa-160">AngularJS supports two-way data binding where data from a model is kept in synchronization with a view template at all times.</span></span> <span data-ttu-id="2dffa-161">As alterações para o modo de exibição são refletidas automaticamente no modelo.</span><span class="sxs-lookup"><span data-stu-id="2dffa-161">Any changes to the view are automatically reflected in the model.</span></span> <span data-ttu-id="2dffa-162">Da mesma forma, as alterações no modelo são refletidas no modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="2dffa-162">Likewise, any changes in the model are reflected in the view.</span></span>

<span data-ttu-id="2dffa-163">Criar um arquivo HTML ou uma ação do controlador com um modo de exibição que acompanha denominado `Databinding`.</span><span class="sxs-lookup"><span data-stu-id="2dffa-163">Create either an HTML file or a controller action with an accompanying view named `Databinding`.</span></span> <span data-ttu-id="2dffa-164">Inclua o seguinte no modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="2dffa-164">Include the following in the view:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

<span data-ttu-id="2dffa-165">Observe que você pode exibir valores de modelo usando diretivas ou dados de associação (`ng-bind`).</span><span class="sxs-lookup"><span data-stu-id="2dffa-165">Notice that you can display model values using either directives or data binding (`ng-bind`).</span></span> <span data-ttu-id="2dffa-166">A página resultante deve ter esta aparência:</span><span class="sxs-lookup"><span data-stu-id="2dffa-166">The resulting page should look like this:</span></span>

![Associação de dados Simple](angular/_static/simple-databinding.png)

### <a name="templates"></a><span data-ttu-id="2dffa-168">Modelos</span><span class="sxs-lookup"><span data-stu-id="2dffa-168">Templates</span></span>

<span data-ttu-id="2dffa-169">[Modelos de](https://docs.angularjs.org/guide/templates) em AngularJS são apenas páginas em HTML decoradas com artefatos e diretivas de AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2dffa-169">[Templates](https://docs.angularjs.org/guide/templates) in AngularJS are just plain HTML pages decorated with AngularJS directives and artifacts.</span></span> <span data-ttu-id="2dffa-170">Um modelo em AngularJS é uma combinação de diretivas, expressões, filtros e controles que combinam com HTML para a exibição de formulário.</span><span class="sxs-lookup"><span data-stu-id="2dffa-170">A template in AngularJS is a mixture of directives, expressions, filters, and controls that combine with HTML to form the view.</span></span>

<span data-ttu-id="2dffa-171">Adicionar outro modo de exibição para demonstrar os modelos e adicione o seguinte para ele:</span><span class="sxs-lookup"><span data-stu-id="2dffa-171">Add another view to demonstrate templates, and add the following to it:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

<span data-ttu-id="2dffa-172">O modelo tem diretivas AngularJS como `ng-app`, `ng-init`, `ng-model` e sintaxe de expressão de associação de dados para associar o `personName` propriedade.</span><span class="sxs-lookup"><span data-stu-id="2dffa-172">The template has AngularJS directives like `ng-app`, `ng-init`, `ng-model` and data binding expression syntax to bind the `personName` property.</span></span> <span data-ttu-id="2dffa-173">Em execução no navegador, a exibição é semelhante a captura de tela abaixo:</span><span class="sxs-lookup"><span data-stu-id="2dffa-173">Running in the browser, the view looks like the screenshot below:</span></span>

![Exemplo de modelos simples 1](angular/_static/simple-templates-1.png)

<span data-ttu-id="2dffa-175">Se você alterar o nome digitando-o no campo de entrada, você verá o texto ao lado do campo de entrada dinamicamente atualização, mostrando a associação de dados bidirecional Angular em ação.</span><span class="sxs-lookup"><span data-stu-id="2dffa-175">If you change the name by typing in the input field, you will see the text next to the input field dynamically update, showing Angular two-way data binding in action.</span></span>

![Exemplo de modelos simples 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a><span data-ttu-id="2dffa-177">Expressões</span><span class="sxs-lookup"><span data-stu-id="2dffa-177">Expressions</span></span>

<span data-ttu-id="2dffa-178">[Expressões](https://docs.angularjs.org/guide/expression) AngularJS são trechos de código JavaScript que são gravados dentro de `{{ expression }}` sintaxe.</span><span class="sxs-lookup"><span data-stu-id="2dffa-178">[Expressions](https://docs.angularjs.org/guide/expression) in AngularJS are JavaScript-like code snippets that are written inside the `{{ expression }}` syntax.</span></span> <span data-ttu-id="2dffa-179">Os dados a partir dessas expressões estão associados a HTML da mesma maneira que `ng-bind` diretivas.</span><span class="sxs-lookup"><span data-stu-id="2dffa-179">The data from these expressions is bound to HTML the same way as `ng-bind` directives.</span></span> <span data-ttu-id="2dffa-180">A principal diferença entre AngularJS expressões e expressões regulares do JavaScript é que AngularJS as expressões são avaliadas em relação a `$scope` objeto em AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2dffa-180">The main difference between AngularJS expressions and regular JavaScript expressions is that AngularJS expressions are evaluated against the `$scope` object in AngularJS.</span></span>

<span data-ttu-id="2dffa-181">As expressões de AngularJS no exemplo abaixo ligação `personName` e um simple JavaScript calculado expressão:</span><span class="sxs-lookup"><span data-stu-id="2dffa-181">The AngularJS expressions in the sample below bind `personName` and a simple JavaScript calculated expression:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

<span data-ttu-id="2dffa-182">O exemplo em execução no navegador mostre o `personName` dados e os resultados do cálculo:</span><span class="sxs-lookup"><span data-stu-id="2dffa-182">The example running in the browser displays the `personName` data and the results of the calculation:</span></span>

![Expressões simples](angular/_static/simple-expressions.png)

### <a name="repeaters"></a><span data-ttu-id="2dffa-184">Repetidores</span><span class="sxs-lookup"><span data-stu-id="2dffa-184">Repeaters</span></span>

<span data-ttu-id="2dffa-185">AngularJS de repetição é feita por meio de uma diretiva primitivo chamada `ng-repeat`.</span><span class="sxs-lookup"><span data-stu-id="2dffa-185">Repeating in AngularJS is done via a primitive directive called `ng-repeat`.</span></span> <span data-ttu-id="2dffa-186">O `ng-repeat` diretiva repete um determinado elemento HTML em uma exibição sobre o tamanho de uma matriz de dados repetidos.</span><span class="sxs-lookup"><span data-stu-id="2dffa-186">The `ng-repeat` directive repeats a given HTML element in a view over the length of a repeating data array.</span></span> <span data-ttu-id="2dffa-187">Repetidores em AngularJS podem repetir em uma matriz de cadeias de caracteres ou objetos.</span><span class="sxs-lookup"><span data-stu-id="2dffa-187">Repeaters in AngularJS can repeat over an array of strings or objects.</span></span> <span data-ttu-id="2dffa-188">Aqui está um exemplo de uso de repetição em uma matriz de cadeias de caracteres:</span><span class="sxs-lookup"><span data-stu-id="2dffa-188">Here is a sample usage of repeating over an array of strings:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

<span data-ttu-id="2dffa-189">O [diretiva repeat](https://docs.angularjs.org/api/ng/directive/ngRepeat) gera uma série de itens de lista em uma lista não ordenada, como você pode ver nas ferramentas de desenvolvedor mostradas nesta captura de tela:</span><span class="sxs-lookup"><span data-stu-id="2dffa-189">The [repeat directive](https://docs.angularjs.org/api/ng/directive/ngRepeat) outputs a series of list items in an unordered list, as you can see in the developer tools shown in this screenshot:</span></span>

![Exemplo de Repetidor](angular/_static/repeater.png)

<span data-ttu-id="2dffa-191">Aqui está um exemplo que se repete em uma matriz de objetos.</span><span class="sxs-lookup"><span data-stu-id="2dffa-191">Here is an example that repeats over an array of objects.</span></span> <span data-ttu-id="2dffa-192">O `ng-init` diretiva estabelece um `names` matriz, onde cada elemento é um objeto que contém primeiro e último nome.</span><span class="sxs-lookup"><span data-stu-id="2dffa-192">The `ng-init` directive establishes a `names` array, where each element is an object containing first and last names.</span></span> <span data-ttu-id="2dffa-193">O `ng-repeat` atribuição, `name in names`, gera um item de lista para cada elemento da matriz.</span><span class="sxs-lookup"><span data-stu-id="2dffa-193">The `ng-repeat` assignment, `name in names`, outputs a list item for every array element.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

<span data-ttu-id="2dffa-194">Nesse caso a saída é igual ao exemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="2dffa-194">The output in this case is the same as in the previous example.</span></span>

<span data-ttu-id="2dffa-195">Angular fornece algumas diretivas adicionais que podem ajudar a fornecer um comportamento com base em onde o loop está em execução.</span><span class="sxs-lookup"><span data-stu-id="2dffa-195">Angular provides some additional directives that can help provide behavior based on where the loop is in its execution.</span></span>

`$index`

<span data-ttu-id="2dffa-196">Use `$index` no `ng-repeat` loop para determinar qual índice posicionar o loop no momento é no.</span><span class="sxs-lookup"><span data-stu-id="2dffa-196">Use `$index` in the `ng-repeat` loop to determine which index position your loop currently is on.</span></span>

<span data-ttu-id="2dffa-197">`$even` e `$odd`</span><span class="sxs-lookup"><span data-stu-id="2dffa-197">`$even` and `$odd`</span></span>

<span data-ttu-id="2dffa-198">Use `$even` no `ng-repeat` loop para determinar se o índice atual em seu loop é uma linha até mesmo indexada.</span><span class="sxs-lookup"><span data-stu-id="2dffa-198">Use `$even` in the `ng-repeat` loop to determine whether the current index in your loop is an even indexed row.</span></span> <span data-ttu-id="2dffa-199">Da mesma forma, use `$odd` para determinar se o índice atual é uma linha indexada ímpar.</span><span class="sxs-lookup"><span data-stu-id="2dffa-199">Similarly, use `$odd` to determine if the current index is an odd indexed row.</span></span>

<span data-ttu-id="2dffa-200">`$first` e `$last`</span><span class="sxs-lookup"><span data-stu-id="2dffa-200">`$first` and `$last`</span></span>

<span data-ttu-id="2dffa-201">Use `$first` no `ng-repeat` loop para determinar se o índice atual em seu loop é a primeira linha.</span><span class="sxs-lookup"><span data-stu-id="2dffa-201">Use `$first` in the `ng-repeat` loop to determine whether the current index in your loop is the first row.</span></span> <span data-ttu-id="2dffa-202">Da mesma forma, use `$last` para determinar se o índice atual é a última linha.</span><span class="sxs-lookup"><span data-stu-id="2dffa-202">Similarly, use `$last` to determine if the current index is the last row.</span></span>

<span data-ttu-id="2dffa-203">Abaixo está um exemplo que mostra `$index`, `$even`, `$odd`, `$first`, e `$last` em ação:</span><span class="sxs-lookup"><span data-stu-id="2dffa-203">Below is a sample that shows `$index`, `$even`, `$odd`, `$first`, and `$last` in action:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

<span data-ttu-id="2dffa-204">Aqui está a saída resultante:</span><span class="sxs-lookup"><span data-stu-id="2dffa-204">Here is the resulting output:</span></span>

![Exemplo de Repetidor 2](angular/_static/repeaters2.png)

### <a name="scope"></a><span data-ttu-id="2dffa-206">$scope</span><span class="sxs-lookup"><span data-stu-id="2dffa-206">$scope</span></span>

<span data-ttu-id="2dffa-207">`$scope`é um objeto JavaScript que atua como união entre o modo de exibição (modelo) e o controlador (explicado abaixo).</span><span class="sxs-lookup"><span data-stu-id="2dffa-207">`$scope` is a JavaScript object that acts as glue between the view (template) and the controller (explained below).</span></span> <span data-ttu-id="2dffa-208">Um modelo de exibição em AngularJS só conhece os valores anexados para o `$scope` objeto no controlador.</span><span class="sxs-lookup"><span data-stu-id="2dffa-208">A view template in AngularJS only knows about the values attached to the `$scope` object in the controller.</span></span>

> [!NOTE]
> <span data-ttu-id="2dffa-209">No mundo MVVM, o `$scope` objeto em AngularJS geralmente é definido como o ViewModel.</span><span class="sxs-lookup"><span data-stu-id="2dffa-209">In the MVVM world, the `$scope` object in AngularJS is often defined as the ViewModel.</span></span> <span data-ttu-id="2dffa-210">A equipe do AngularJS refere-se para o `$scope` objeto como o modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="2dffa-210">The AngularJS team refers to the `$scope` object as the Data-Model.</span></span> <span data-ttu-id="2dffa-211">[Saiba mais sobre os escopos em AngularJS](https://docs.angularjs.org/guide/scope).</span><span class="sxs-lookup"><span data-stu-id="2dffa-211">[Learn more about Scopes in AngularJS](https://docs.angularjs.org/guide/scope).</span></span>

<span data-ttu-id="2dffa-212">Abaixo está um exemplo simples que mostra como definir propriedades em `$scope` dentro de um arquivo separado do JavaScript, *scope.js*:</span><span class="sxs-lookup"><span data-stu-id="2dffa-212">Below is a simple example showing how to set properties on `$scope` within a separate JavaScript file, *scope.js*:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

<span data-ttu-id="2dffa-213">Observe o `$scope` parâmetro passado para o controlador na linha 2.</span><span class="sxs-lookup"><span data-stu-id="2dffa-213">Observe the `$scope` parameter passed to the controller on line 2.</span></span> <span data-ttu-id="2dffa-214">Este objeto é o que o modo de exibição conhece.</span><span class="sxs-lookup"><span data-stu-id="2dffa-214">This object is what the view knows about.</span></span> <span data-ttu-id="2dffa-215">Na linha 3, uma propriedade chamada "name" para "Jane Mary" está sendo configurado.</span><span class="sxs-lookup"><span data-stu-id="2dffa-215">On line 3, we are setting a property called "name" to "Mary Jane".</span></span>

<span data-ttu-id="2dffa-216">O que acontece quando uma determinada propriedade não é encontrada pela exibição?</span><span class="sxs-lookup"><span data-stu-id="2dffa-216">What happens when a particular property is not found by the view?</span></span> <span data-ttu-id="2dffa-217">O modo de exibição definido a seguir se refere às propriedades de "nome" e "Idade":</span><span class="sxs-lookup"><span data-stu-id="2dffa-217">The view defined below refers to "name" and "age" properties:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

<span data-ttu-id="2dffa-218">Observe na linha 9 que estamos pedindo Angular para mostrar a propriedade "name" usando a sintaxe de expressão.</span><span class="sxs-lookup"><span data-stu-id="2dffa-218">Notice on line 9 that we are asking Angular to show the "name" property using expression syntax.</span></span> <span data-ttu-id="2dffa-219">Linha de 10, em seguida, faz referência a "Idade", uma propriedade que não existe.</span><span class="sxs-lookup"><span data-stu-id="2dffa-219">Line 10 then refers to "age", a property that does not exist.</span></span> <span data-ttu-id="2dffa-220">O exemplo de execução mostra o nome definido como "Mary Jane" e nada para idade.</span><span class="sxs-lookup"><span data-stu-id="2dffa-220">The running example shows the name set to "Mary Jane" and nothing for age.</span></span> <span data-ttu-id="2dffa-221">Propriedades ausentes são ignoradas.</span><span class="sxs-lookup"><span data-stu-id="2dffa-221">Missing properties are ignored.</span></span>

![Exemplo de escopo](angular/_static/scope.png)

### <a name="modules"></a><span data-ttu-id="2dffa-223">Módulos</span><span class="sxs-lookup"><span data-stu-id="2dffa-223">Modules</span></span>

<span data-ttu-id="2dffa-224">Um [módulo](https://docs.angularjs.org/guide/module) em AngularJS é uma coleção de controladores, serviços, diretivas, etc. O `angular.module()` chamada de função é usada para criar, registrar e recuperar os módulos em AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2dffa-224">A [module](https://docs.angularjs.org/guide/module) in AngularJS is a collection of controllers, services, directives, etc. The `angular.module()` function call is used to create, register, and retrieve modules in AngularJS.</span></span> <span data-ttu-id="2dffa-225">Todos os módulos, incluindo aqueles fornecidos pela equipe de AngularJS e bibliotecas de terceiros, devem ser registrados com o `angular.module()` função.</span><span class="sxs-lookup"><span data-stu-id="2dffa-225">All modules, including those shipped by the AngularJS team and third party libraries, should be registered using the `angular.module()` function.</span></span>

<span data-ttu-id="2dffa-226">Abaixo está um trecho de código que mostra como criar um novo módulo no AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2dffa-226">Below is a snippet of code that shows how to create a new module in AngularJS.</span></span> <span data-ttu-id="2dffa-227">O primeiro parâmetro é o nome do módulo.</span><span class="sxs-lookup"><span data-stu-id="2dffa-227">The first parameter is the name of the module.</span></span> <span data-ttu-id="2dffa-228">O segundo parâmetro define dependências em outros módulos.</span><span class="sxs-lookup"><span data-stu-id="2dffa-228">The second parameter defines dependencies on other modules.</span></span> <span data-ttu-id="2dffa-229">Neste artigo, podemos ser exibida como transmitir essas dependências para um `angular.module()` chamada de método.</span><span class="sxs-lookup"><span data-stu-id="2dffa-229">Later in this article, we will be showing how to pass these dependencies to an `angular.module()` method call.</span></span>

```javascript
var personApp = angular.module('personApp', []);
```

<span data-ttu-id="2dffa-230">Use o `ng-app` diretiva para representar um módulo AngularJS na página.</span><span class="sxs-lookup"><span data-stu-id="2dffa-230">Use the `ng-app` directive to represent an AngularJS module on the page.</span></span> <span data-ttu-id="2dffa-231">Para usar um módulo, atribuir o nome do módulo, `personApp` neste exemplo, para o `ng-app` diretiva em nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="2dffa-231">To use a module, assign the name of the module, `personApp` in this example, to the `ng-app` directive in our template.</span></span>

```html
<body ng-app="personApp">
```

### <a name="controllers"></a><span data-ttu-id="2dffa-232">Controladores</span><span class="sxs-lookup"><span data-stu-id="2dffa-232">Controllers</span></span>

<span data-ttu-id="2dffa-233">[Controladores](https://docs.angularjs.org/guide/controller) AngularJS são o primeiro ponto de entrada para seu código.</span><span class="sxs-lookup"><span data-stu-id="2dffa-233">[Controllers](https://docs.angularjs.org/guide/controller) in AngularJS are the first point of entry for your code.</span></span> <span data-ttu-id="2dffa-234">O `<module name>.controller()` chamada de função é usada para criar e registrar os controladores em AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2dffa-234">The `<module name>.controller()` function call is used to create and register controllers in AngularJS.</span></span> <span data-ttu-id="2dffa-235">O `ng-controller` diretiva é usada para representar um controlador AngularJS na página HTML.</span><span class="sxs-lookup"><span data-stu-id="2dffa-235">The `ng-controller` directive is used to represent an AngularJS controller on the HTML page.</span></span> <span data-ttu-id="2dffa-236">A função do controlador no Angular é definir o estado e o comportamento do modelo de dados (`$scope`).</span><span class="sxs-lookup"><span data-stu-id="2dffa-236">The role of the controller in Angular is to set state and behavior of the data model (`$scope`).</span></span> <span data-ttu-id="2dffa-237">Controladores não devem ser usados para manipular o DOM diretamente.</span><span class="sxs-lookup"><span data-stu-id="2dffa-237">Controllers should not be used to manipulate the DOM directly.</span></span>

<span data-ttu-id="2dffa-238">Abaixo está um trecho de código que registra um novo controlador.</span><span class="sxs-lookup"><span data-stu-id="2dffa-238">Below is a snippet of code that registers a new controller.</span></span> <span data-ttu-id="2dffa-239">O `personApp` variável no trecho faz referência a um módulo Angular, que é definido na linha 2.</span><span class="sxs-lookup"><span data-stu-id="2dffa-239">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

<span data-ttu-id="2dffa-240">O modo de exibição usando o `ng-controller` diretiva atribui o nome do controlador:</span><span class="sxs-lookup"><span data-stu-id="2dffa-240">The view using the `ng-controller` directive assigns the controller name:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

<span data-ttu-id="2dffa-241">A página mostra "Mary" e "Jane" que correspondem do `firstName` e `lastName` propriedades anexadas para o `$scope` objeto:</span><span class="sxs-lookup"><span data-stu-id="2dffa-241">The page shows "Mary" and "Jane" that correspond to the `firstName` and `lastName` properties attached to the `$scope` object:</span></span>

![Exemplo de controlador](angular/_static/controllers.png)

### <a name="components"></a><span data-ttu-id="2dffa-243">Componentes</span><span class="sxs-lookup"><span data-stu-id="2dffa-243">Components</span></span>

<span data-ttu-id="2dffa-244">[Componentes](https://docs.angularjs.org/guide/component) em Angular 1.5. x permitir o encapsulamento e a capacidade de criar elementos HTML individuais.</span><span class="sxs-lookup"><span data-stu-id="2dffa-244">[Components](https://docs.angularjs.org/guide/component) in Angular 1.5.x allow for the encapsulation and capability of creating individual HTML elements.</span></span> <span data-ttu-id="2dffa-245">1.4 Angular você obteria o mesmo recurso usando o método .directive().</span><span class="sxs-lookup"><span data-stu-id="2dffa-245">In Angular 1.4.x you could achieve the same feature using the .directive() method.</span></span>

<span data-ttu-id="2dffa-246">Usando o método .component(), desenvolvimento é simplificado obter a funcionalidade da diretiva e o controlador.</span><span class="sxs-lookup"><span data-stu-id="2dffa-246">By using the .component() method, development is simplified gaining the functionality of the directive and the controller.</span></span> <span data-ttu-id="2dffa-247">Outros benefícios incluem; isolamento de escopo, as práticas recomendadas são inerentes e migração para 2 Angular torna-se uma tarefa mais fácil.</span><span class="sxs-lookup"><span data-stu-id="2dffa-247">Other benefits include; scope isolation, best practices are inherent, and migration to Angular 2 becomes an easier task.</span></span> <span data-ttu-id="2dffa-248">O `<module name>.component()` chamada de função é usada para criar e registrar componentes na AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2dffa-248">The `<module name>.component()` function call is used to create and register components in AngularJS.</span></span>

<span data-ttu-id="2dffa-249">Abaixo está um trecho de código que registra um novo componente.</span><span class="sxs-lookup"><span data-stu-id="2dffa-249">Below is a snippet of code that registers a new component.</span></span> <span data-ttu-id="2dffa-250">O `personApp` variável no trecho faz referência a um módulo Angular, que é definido na linha 2.</span><span class="sxs-lookup"><span data-stu-id="2dffa-250">The `personApp` variable in the snippet references an Angular module, which is defined on line 2.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

<span data-ttu-id="2dffa-251">O modo de exibição onde podemos estiver exibindo o elemento HTML personalizado.</span><span class="sxs-lookup"><span data-stu-id="2dffa-251">The view where we are displaying the custom HTML element.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

<span data-ttu-id="2dffa-252">O modelo associado usado pelo componente:</span><span class="sxs-lookup"><span data-stu-id="2dffa-252">The associated template used by component:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

<span data-ttu-id="2dffa-253">A página mostra "Aftab" e "Ansari" que correspondem do `firstName` e `lastName` propriedades anexadas para o `vm` objeto:</span><span class="sxs-lookup"><span data-stu-id="2dffa-253">The page shows "Aftab" and "Ansari" that correspond to the `firstName` and `lastName` properties attached to the `vm` object:</span></span>

![Exemplo de componentes](angular/_static/components.png)

### <a name="services"></a><span data-ttu-id="2dffa-255">Serviços</span><span class="sxs-lookup"><span data-stu-id="2dffa-255">Services</span></span>

<span data-ttu-id="2dffa-256">[Serviços](https://docs.angularjs.org/guide/services) em AngularJS normalmente são usados para código compartilhado abstraído em um arquivo que pode ser usado em todo o tempo de vida de um aplicativo Angular.</span><span class="sxs-lookup"><span data-stu-id="2dffa-256">[Services](https://docs.angularjs.org/guide/services) in AngularJS are commonly used for shared code that is abstracted away into a file which can be used throughout the lifetime of an Angular application.</span></span> <span data-ttu-id="2dffa-257">Serviços são instanciados lentamente, o que significa que não haverá uma instância de um serviço, a menos que um componente que depende do serviço é usado.</span><span class="sxs-lookup"><span data-stu-id="2dffa-257">Services are lazily instantiated, meaning that there will not be an instance of a service unless a component that depends on the service gets used.</span></span> <span data-ttu-id="2dffa-258">Fábricas são um exemplo de um serviço usado em aplicativos de AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2dffa-258">Factories are an example of a service used in AngularJS applications.</span></span> <span data-ttu-id="2dffa-259">Fábricas são criadas usando o `myApp.factory()` função chamada, onde `myApp` é o módulo.</span><span class="sxs-lookup"><span data-stu-id="2dffa-259">Factories are created using the `myApp.factory()` function call, where `myApp` is the module.</span></span>

<span data-ttu-id="2dffa-260">Abaixo está um exemplo que mostra como usar as fábricas em AngularJS:</span><span class="sxs-lookup"><span data-stu-id="2dffa-260">Below is an example that shows how to use factories in AngularJS:</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

<span data-ttu-id="2dffa-261">Para chamar esta fábrica do controlador, transmita `personFactory` como um parâmetro para o `controller` função:</span><span class="sxs-lookup"><span data-stu-id="2dffa-261">To call this factory from the controller, pass `personFactory` as a parameter to the `controller` function:</span></span>

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a><span data-ttu-id="2dffa-262">Usando serviços de falar com um ponto de extremidade REST</span><span class="sxs-lookup"><span data-stu-id="2dffa-262">Using services to talk to a REST endpoint</span></span>

<span data-ttu-id="2dffa-263">Abaixo está um exemplo de ponta a ponta usando serviços em AngularJS para interagir com um ponto de extremidade de API da Web do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2dffa-263">Below is an end-to-end example using services in AngularJS to interact with an ASP.NET Core Web API endpoint.</span></span> <span data-ttu-id="2dffa-264">O exemplo obtém os dados da API da Web e exibe os dados em um modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="2dffa-264">The example gets data from the Web API and displays the data in a view template.</span></span> <span data-ttu-id="2dffa-265">Vamos começar com o modo de exibição primeiro:</span><span class="sxs-lookup"><span data-stu-id="2dffa-265">Let's start with the view first:</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

<span data-ttu-id="2dffa-266">Nesta exibição, temos um módulo Angular chamado `PersonsApp` e um controlador chamado `personController`.</span><span class="sxs-lookup"><span data-stu-id="2dffa-266">In this view, we have an Angular module called `PersonsApp` and a controller called `personController`.</span></span> <span data-ttu-id="2dffa-267">Estamos usando `ng-repeat` para iterar sobre a lista de pessoas.</span><span class="sxs-lookup"><span data-stu-id="2dffa-267">We are using `ng-repeat` to iterate over the list of persons.</span></span> <span data-ttu-id="2dffa-268">Podemos faz referência três arquivos JavaScript personalizados em linhas 17-19.</span><span class="sxs-lookup"><span data-stu-id="2dffa-268">We are referencing three custom JavaScript files on lines 17-19.</span></span>

<span data-ttu-id="2dffa-269">O *personApp.js* arquivo é usado para registrar o `PersonsApp` módulo; e a sintaxe é semelhante aos exemplos anteriores.</span><span class="sxs-lookup"><span data-stu-id="2dffa-269">The *personApp.js* file is used to register the `PersonsApp` module; and, the syntax is similar to previous examples.</span></span> <span data-ttu-id="2dffa-270">Estamos usando o `angular.module` função para criar uma nova instância do módulo que trabalharemos com.</span><span class="sxs-lookup"><span data-stu-id="2dffa-270">We are using the `angular.module` function to create a new instance of the module that we will be working with.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

<span data-ttu-id="2dffa-271">Vamos dar uma olhada *personFactory.js*, abaixo.</span><span class="sxs-lookup"><span data-stu-id="2dffa-271">Let's take a look at *personFactory.js*, below.</span></span> <span data-ttu-id="2dffa-272">Estamos ligando para o módulo `factory` método para criar uma fábrica.</span><span class="sxs-lookup"><span data-stu-id="2dffa-272">We are calling the module’s `factory` method to create a factory.</span></span> <span data-ttu-id="2dffa-273">Linha de 12 mostra o Angular interna `$http` recuperar informações sobre as pessoas de um serviço web do serviço.</span><span class="sxs-lookup"><span data-stu-id="2dffa-273">Line 12 shows the built-in Angular `$http` service retrieving people information from a web service.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

<span data-ttu-id="2dffa-274">Em *personController.js*, estamos ligando para o módulo `controller` método para criar o controlador.</span><span class="sxs-lookup"><span data-stu-id="2dffa-274">In *personController.js*, we are calling the module’s `controller` method to create the controller.</span></span> <span data-ttu-id="2dffa-275">O `$scope` do objeto `people` propriedade recebe os dados retornados do personFactory (linha 13).</span><span class="sxs-lookup"><span data-stu-id="2dffa-275">The `$scope` object's `people` property is assigned the data returned from the personFactory (line 13).</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

<span data-ttu-id="2dffa-276">Vamos dar uma olhada rápida a API da Web e o modelo por trás dele.</span><span class="sxs-lookup"><span data-stu-id="2dffa-276">Let's take a quick look at the Web API and the model behind it.</span></span> <span data-ttu-id="2dffa-277">O `Person` modelo é um POCO (objeto Plain Old CLR) com `Id`, `FirstName`, e `LastName` propriedades:</span><span class="sxs-lookup"><span data-stu-id="2dffa-277">The `Person` model is a POCO (Plain Old CLR Object) with `Id`, `FirstName`, and `LastName` properties:</span></span>

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

<span data-ttu-id="2dffa-278">O `Person` retorna uma lista formatada em JSON de `Person` objetos:</span><span class="sxs-lookup"><span data-stu-id="2dffa-278">The `Person` controller returns a JSON-formatted list of `Person` objects:</span></span>

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

<span data-ttu-id="2dffa-279">Vamos ver o aplicativo em ação:</span><span class="sxs-lookup"><span data-stu-id="2dffa-279">Let's see the application in action:</span></span>

![Exibindo resultados REST controlador](angular/_static/rest-bound.png)

<span data-ttu-id="2dffa-281">Você pode [exibir a estrutura do aplicativo no GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span><span class="sxs-lookup"><span data-stu-id="2dffa-281">You can [view the application's structure on GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

> [!NOTE]
> <span data-ttu-id="2dffa-282">Para obter mais informações sobre como estruturar AngularJS aplicativos, consulte [guia de estilo do John Papa Angular](https://github.com/johnpapa/angular-styleguide)</span><span class="sxs-lookup"><span data-stu-id="2dffa-282">For more on structuring AngularJS applications, see [John Papa's Angular Style Guide](https://github.com/johnpapa/angular-styleguide)</span></span>

&nbsp;

> [!NOTE]
> <span data-ttu-id="2dffa-283">Para criar o módulo AngularJS, controlador, fábrica, arquivos de diretiva e exibir facilmente, certifique-se fazer check-out do Hashimi Sayed [SideWaffle o pacote de modelo para o Visual Studio](http://sidewaffle.com/).</span><span class="sxs-lookup"><span data-stu-id="2dffa-283">To create AngularJS module, controller, factory, directive and view files easily, be sure to check out Sayed Hashimi's [SideWaffle template pack for Visual Studio](http://sidewaffle.com/).</span></span> <span data-ttu-id="2dffa-284">Hashimi sayed é gerente de programas sênior da equipe do Web Visual Studio na Microsoft e SideWaffle modelos são considerados o padrão.</span><span class="sxs-lookup"><span data-stu-id="2dffa-284">Sayed Hashimi is a Senior Program Manager on the Visual Studio Web Team at Microsoft and SideWaffle templates are considered the gold standard.</span></span> <span data-ttu-id="2dffa-285">No momento da redação deste artigo, SideWaffle está disponível para o Visual Studio 2012, 2013 e 2015.</span><span class="sxs-lookup"><span data-stu-id="2dffa-285">At the time of this writing, SideWaffle is available for Visual Studio 2012, 2013, and 2015.</span></span>

### <a name="routing-and-multiple-views"></a><span data-ttu-id="2dffa-286">Roteamento e vários modos de exibição</span><span class="sxs-lookup"><span data-stu-id="2dffa-286">Routing and multiple views</span></span>

<span data-ttu-id="2dffa-287">AngularJS tem um provedor de rota interna para tratar SPA (aplicativo de página única) com base em navegação.</span><span class="sxs-lookup"><span data-stu-id="2dffa-287">AngularJS has a built-in route provider to handle SPA (Single Page Application) based navigation.</span></span> <span data-ttu-id="2dffa-288">Para trabalhar com roteamento em AngularJS, você deve adicionar o `angular-route` biblioteca usando o Bower.</span><span class="sxs-lookup"><span data-stu-id="2dffa-288">To work with routing in AngularJS, you must add the `angular-route` library using Bower.</span></span> <span data-ttu-id="2dffa-289">Você pode ver no [bower. JSON](#angular-bower-json) arquivo referenciado no início deste artigo que nós são já faz referência a ele em nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="2dffa-289">You can see in the [bower.json](#angular-bower-json) file referenced at the start of this article that we are already referencing it in our project.</span></span>

<span data-ttu-id="2dffa-290">Depois de instalar o pacote, adicione a referência de script (*route.js angular*) ao modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="2dffa-290">After you install the package, add the script reference (*angular-route.js*) to your view.</span></span>

<span data-ttu-id="2dffa-291">Agora vamos dar o aplicativo de pessoa é construído e adicionar navegação a ele.</span><span class="sxs-lookup"><span data-stu-id="2dffa-291">Now let's take the Person App we have been building and add navigation to it.</span></span> <span data-ttu-id="2dffa-292">Primeiro, faremos uma cópia do aplicativo, criando um novo `PeopleController` ação chamada `Spa` e correspondente `Spa.cshtml` exibição copiando o modo de exibição cshtml o `People` pasta.</span><span class="sxs-lookup"><span data-stu-id="2dffa-292">First, we will make a copy of the app by creating a new `PeopleController` action called `Spa` and a corresponding `Spa.cshtml` view by copying the Index.cshtml view in the `People` folder.</span></span> <span data-ttu-id="2dffa-293">Adicione uma referência de script para `angular-route` (consulte a linha 11).</span><span class="sxs-lookup"><span data-stu-id="2dffa-293">Add a script reference to `angular-route` (see line 11).</span></span> <span data-ttu-id="2dffa-294">Também adicionar uma `div` marcado com o `ng-view` diretiva (consulte a linha 6) como um espaço reservado para colocar os modos de exibição no.</span><span class="sxs-lookup"><span data-stu-id="2dffa-294">Also add a `div` marked with the `ng-view` directive (see line 6) as a placeholder to place views in.</span></span> <span data-ttu-id="2dffa-295">Vamos usar vários adicionais *. js* arquivos que são referenciados nas linhas de 13 a 16.</span><span class="sxs-lookup"><span data-stu-id="2dffa-295">We are going to be using several additional *.js* files which are referenced on lines 13-16.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

<span data-ttu-id="2dffa-296">Vamos dar uma olhada *personModule.js* arquivo para ver como podemos são instanciar o módulo com o roteamento.</span><span class="sxs-lookup"><span data-stu-id="2dffa-296">Let's take a look at *personModule.js* file to see how we are instantiating the module with routing.</span></span> <span data-ttu-id="2dffa-297">Passa `ngRoute` como uma biblioteca no módulo.</span><span class="sxs-lookup"><span data-stu-id="2dffa-297">We are passing `ngRoute` as a library into the module.</span></span> <span data-ttu-id="2dffa-298">Este módulo manipula o roteamento em nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2dffa-298">This module handles routing in our application.</span></span>

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

<span data-ttu-id="2dffa-299">O *personRoutes.js* arquivo, abaixo, define rotas com base no provedor de rota.</span><span class="sxs-lookup"><span data-stu-id="2dffa-299">The *personRoutes.js* file, below, defines routes based on the route provider.</span></span> <span data-ttu-id="2dffa-300">Linhas 4 a 7 definem navegação efetivamente dizendo, quando uma URL com `/persons` é solicitada, usar um modelo chamado `partials/personlist` trabalhando `personListController`.</span><span class="sxs-lookup"><span data-stu-id="2dffa-300">Lines 4-7 define navigation by effectively saying, when a URL with `/persons` is requested, use a template called `partials/personlist` by working through `personListController`.</span></span> <span data-ttu-id="2dffa-301">Linhas de 8 a 11 indicam uma página de detalhes com um parâmetro de rota do `personId`.</span><span class="sxs-lookup"><span data-stu-id="2dffa-301">Lines 8-11 indicate a detail page with a route parameter of `personId`.</span></span> <span data-ttu-id="2dffa-302">Se a URL não coincidir com um dos padrões, Angular assume como padrão o `/persons` exibição.</span><span class="sxs-lookup"><span data-stu-id="2dffa-302">If the URL doesn't match one of the patterns, Angular defaults to the `/persons` view.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

<span data-ttu-id="2dffa-303">O `personlist.html` arquivo é uma exibição parcial que contém apenas o HTML necessário para exibir a lista de pessoas.</span><span class="sxs-lookup"><span data-stu-id="2dffa-303">The `personlist.html` file is a partial view containing only the HTML needed to display person list.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

<span data-ttu-id="2dffa-304">O controlador é definido usando o módulo `controller` funcionar em *personListController.js*.</span><span class="sxs-lookup"><span data-stu-id="2dffa-304">The controller is defined by using the module's `controller` function in *personListController.js*.</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

<span data-ttu-id="2dffa-305">Se executar este aplicativo e navegue até o `people/spa#/persons` URL, veremos:</span><span class="sxs-lookup"><span data-stu-id="2dffa-305">If we run this application and navigate to the `people/spa#/persons` URL, we will see:</span></span>

![Exibição de lista de pessoas](angular/_static/spa-persons.png)

<span data-ttu-id="2dffa-307">Se, navegue até uma página de detalhes, por exemplo `people/spa#/persons/2`, veremos a exibição parcial detalhes:</span><span class="sxs-lookup"><span data-stu-id="2dffa-307">If we navigate to a detail page, for example `people/spa#/persons/2`, we will see the detail partial view:</span></span>

![Exibição de detalhes da pessoa](angular/_static/spa-persons-2.png)

<span data-ttu-id="2dffa-309">Você pode exibir o código-fonte completo e todos os arquivos não mostrados neste artigo em [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span><span class="sxs-lookup"><span data-stu-id="2dffa-309">You can view the full source and any files not shown in this article on [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).</span></span>

### <a name="event-handlers"></a><span data-ttu-id="2dffa-310">Manipuladores de Eventos</span><span class="sxs-lookup"><span data-stu-id="2dffa-310">Event Handlers</span></span>

<span data-ttu-id="2dffa-311">Há um número de diretivas em AngularJS que adiciona recursos de manipulação de eventos para os elementos de entrada no seu HTML DOM.</span><span class="sxs-lookup"><span data-stu-id="2dffa-311">There are a number of directives in AngularJS that add event-handling capabilities to the input elements in your HTML DOM.</span></span> <span data-ttu-id="2dffa-312">Abaixo está uma lista dos eventos que são criados em AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2dffa-312">Below is a list of the events that are built into AngularJS.</span></span>

   * `ng-click`

   * `ng-dbl-click`

   * `ng-mousedown`

   * `ng-mouseup`

   * `ng-mouseenter`

   * `ng-mouseleave`

   * `ng-mousemove`

   * `ng-keydown`

   * `ng-keyup`

   * `ng-keypress`

   * `ng-change`

> [!NOTE]
> <span data-ttu-id="2dffa-313">Você pode adicionar seus próprios manipuladores de eventos usando o [diretivas personalizadas de recursos em AngularJS](https://docs.angularjs.org/guide/directive).</span><span class="sxs-lookup"><span data-stu-id="2dffa-313">You can add your own event handlers using the [custom directives feature in AngularJS](https://docs.angularjs.org/guide/directive).</span></span>

<span data-ttu-id="2dffa-314">Vamos examinar como o `ng-click` evento está conectado.</span><span class="sxs-lookup"><span data-stu-id="2dffa-314">Let's look at how the `ng-click` event is wired up.</span></span> <span data-ttu-id="2dffa-315">Criar um novo arquivo JavaScript chamado *eventHandlerController.js*e adicione o seguinte para ele:</span><span class="sxs-lookup"><span data-stu-id="2dffa-315">Create a new JavaScript file named *eventHandlerController.js*, and add the following to it:</span></span>

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

<span data-ttu-id="2dffa-316">Observe o novo `sayName` funcionar em `eventHandlerController` na linha 5 acima.</span><span class="sxs-lookup"><span data-stu-id="2dffa-316">Notice the new `sayName` function in `eventHandlerController` on line 5 above.</span></span> <span data-ttu-id="2dffa-317">O método All está fazendo para agora está mostrando um alerta de JavaScript para o usuário com uma mensagem de boas-vinda.</span><span class="sxs-lookup"><span data-stu-id="2dffa-317">All the method is doing for now is showing a JavaScript alert to the user with a welcome message.</span></span>

<span data-ttu-id="2dffa-318">O modo de exibição a seguir associa uma função de controlador para um evento AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2dffa-318">The view below binds a controller function to an AngularJS event.</span></span> <span data-ttu-id="2dffa-319">A linha 9 tem um botão no qual o `ng-click` Angular diretiva foi aplicada.</span><span class="sxs-lookup"><span data-stu-id="2dffa-319">Line 9 has a button on which the `ng-click` Angular directive has been applied.</span></span> <span data-ttu-id="2dffa-320">Ele chama nosso `sayName` função, que é anexada ao `$scope` objeto passado para este modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="2dffa-320">It calls our `sayName` function, which is attached to the `$scope` object passed to this view.</span></span>

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

<span data-ttu-id="2dffa-321">O exemplo de execução demonstra que o controlador `sayName` função é chamada automaticamente quando o botão é clicado.</span><span class="sxs-lookup"><span data-stu-id="2dffa-321">The running example demonstrates that the controller's `sayName` function is called automatically when the button is clicked.</span></span>

![Evento de clique](angular/_static/events.png)

<span data-ttu-id="2dffa-323">Para obter mais detalhes sobre as diretivas de manipulador de eventos internos do AngularJS, certifique-se de cabeçalho para o [site de documentação](https://docs.angularjs.org/api/ng/directive/ngClick) do AngularJS.</span><span class="sxs-lookup"><span data-stu-id="2dffa-323">For more detail on AngularJS built-in event handler directives, be sure to head to the [documentation website](https://docs.angularjs.org/api/ng/directive/ngClick) of AngularJS.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2dffa-324">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="2dffa-324">Additional resources</span></span>

* [<span data-ttu-id="2dffa-325">Documentos angulares</span><span class="sxs-lookup"><span data-stu-id="2dffa-325">Angular Docs</span></span>](https://docs.angularjs.org)

* [<span data-ttu-id="2dffa-326">Info 2 angular</span><span class="sxs-lookup"><span data-stu-id="2dffa-326">Angular 2 Info</span></span>](https://angular.io/)

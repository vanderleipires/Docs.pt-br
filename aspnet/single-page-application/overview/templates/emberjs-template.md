---
uid: single-page-application/overview/templates/emberjs-template
title: Modelo de EmberJS | Microsoft Docs
author: xqiu
description: Modelo de EmberJS
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26506795"
---
<a name="emberjs-template"></a><span data-ttu-id="7a779-103">Modelo de EmberJS</span><span class="sxs-lookup"><span data-stu-id="7a779-103">EmberJS template</span></span>
====================
<span data-ttu-id="7a779-104">por [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="7a779-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="7a779-105">O modelo MVC EmberJS é escrito por Xinyang Qiu, Thiago Santos e Nathan Totten.</span><span class="sxs-lookup"><span data-stu-id="7a779-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="7a779-106">Baixe o modelo MVC EmberJS</span><span class="sxs-lookup"><span data-stu-id="7a779-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)


<span data-ttu-id="7a779-107">O modelo EmberJS SPA foi projetado para começar a criar rapidamente aplicativos web interativos de cliente usando EmberJS.</span><span class="sxs-lookup"><span data-stu-id="7a779-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="7a779-108">"Aplicativo de página única" (SPA) é o termo geral para um aplicativo web que carrega uma única página HTML e, em seguida, atualiza a página dinamicamente, em vez de carregar as novas páginas.</span><span class="sxs-lookup"><span data-stu-id="7a779-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="7a779-109">Após o carregamento de página inicial, o SPA se comunica com o servidor por meio de solicitações do AJAX.</span><span class="sxs-lookup"><span data-stu-id="7a779-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="7a779-110">AJAX não é nada novo, mas atualmente há estruturas de JavaScript que tornam mais fácil de criar e manter um grande aplicativo SPA sofisticado.</span><span class="sxs-lookup"><span data-stu-id="7a779-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="7a779-111">Além disso, HTML 5 e CSS3 estão tornando mais fácil criar interfaces do usuário avançados.</span><span class="sxs-lookup"><span data-stu-id="7a779-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="7a779-112">O modelo do SPA EmberJS usa o [Ember](http://emberjs.com/) biblioteca JavaScript para manipular atualizações de página de solicitações do AJAX.</span><span class="sxs-lookup"><span data-stu-id="7a779-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="7a779-113">Ember.js usa associação de dados para sincronizar a página com os dados mais recentes.</span><span class="sxs-lookup"><span data-stu-id="7a779-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="7a779-114">Dessa forma, você não precisa escrever nenhum código que percorre os dados JSON e atualiza o DOM.</span><span class="sxs-lookup"><span data-stu-id="7a779-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="7a779-115">Em vez disso, você deve colocar atributos declarativos no HTML que indicam Ember.js como apresentar os dados.</span><span class="sxs-lookup"><span data-stu-id="7a779-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="7a779-116">No lado do servidor, o modelo EmberJS é quase idêntico de [modelo SPA KnockoutJS](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="7a779-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="7a779-117">Ele usa o ASP.NET MVC para servir de documentos HTML e ASP.NET Web API para lidar com solicitações do AJAX do cliente.</span><span class="sxs-lookup"><span data-stu-id="7a779-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="7a779-118">Para obter mais informações sobre esses aspectos do modelo, consulte o [KnockoutJS modelo](../introduction/knockoutjs-template.md) documentação.</span><span class="sxs-lookup"><span data-stu-id="7a779-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="7a779-119">Este tópico aborda as diferenças entre o modelo de separação e o modelo EmberJS.</span><span class="sxs-lookup"><span data-stu-id="7a779-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="7a779-120">Criar um projeto de modelo do SPA EmberJS</span><span class="sxs-lookup"><span data-stu-id="7a779-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="7a779-121">Baixe e instale o modelo, clique no botão de Download acima.</span><span class="sxs-lookup"><span data-stu-id="7a779-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="7a779-122">Talvez seja necessário reiniciar o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7a779-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="7a779-123">No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="7a779-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="7a779-124">Em **Visual C#**, selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="7a779-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="7a779-125">Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="7a779-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="7a779-126">Nomeie o projeto e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="7a779-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="7a779-127">No **novo projeto** assistente, selecione **Ember.js SPA projeto**.</span><span class="sxs-lookup"><span data-stu-id="7a779-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="7a779-128">Visão geral do modelo EmberJS SPA</span><span class="sxs-lookup"><span data-stu-id="7a779-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="7a779-129">O modelo de EmberJS usa uma combinação de jQuery, Ember.js, Handlebars.js para criar uma interface do usuário interativo e sem interrupções.</span><span class="sxs-lookup"><span data-stu-id="7a779-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="7a779-130">Ember.js é uma biblioteca de JavaScript que usa um padrão MVC do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="7a779-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="7a779-131">Um *modelo*escrito na linguagem de modelagem guidões, descreve a interface de usuário do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7a779-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="7a779-132">No modo de versão, o [compilador Guidões](https://github.com/Myslik/csharp-ember-handlebars) é usado para agrupar e compilar o modelo Guidões.</span><span class="sxs-lookup"><span data-stu-id="7a779-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="7a779-133">Um *modelo* armazena os dados de aplicativo que ele obtém do servidor (listas de tarefas e itens de tarefas).</span><span class="sxs-lookup"><span data-stu-id="7a779-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="7a779-134">Um *controlador* armazena o estado do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7a779-134">A *controller* stores application state.</span></span> <span data-ttu-id="7a779-135">Controladores geralmente apresentam dados de modelo para os modelos correspondentes.</span><span class="sxs-lookup"><span data-stu-id="7a779-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="7a779-136">Um *exibição* converte primitivo eventos do aplicativo e os transmite para o controlador.</span><span class="sxs-lookup"><span data-stu-id="7a779-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="7a779-137">Um *roteador* gerencia o estado do aplicativo, mantendo os modelos e URLs em sincronia.</span><span class="sxs-lookup"><span data-stu-id="7a779-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="7a779-138">Além disso, a biblioteca Ember dados pode ser usada para sincronizar objetos JSON (obtidos do servidor por meio de uma API RESTful) e os modelos de cliente.</span><span class="sxs-lookup"><span data-stu-id="7a779-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="7a779-139">O modelo EmberJS SPA organiza os scripts em oito camadas:</span><span class="sxs-lookup"><span data-stu-id="7a779-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="7a779-140">webapi\_adapter.js, webapi\_serializer.js: estende a biblioteca Ember dados para trabalhar com a API da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7a779-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="7a779-141">Scripts/Helpers.js: Define os auxiliares Ember Guidões novo.</span><span class="sxs-lookup"><span data-stu-id="7a779-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="7a779-142">Scripts/App.js: Cria o aplicativo e configura o adaptador e o serializador.</span><span class="sxs-lookup"><span data-stu-id="7a779-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="7a779-143">Aplicativo/scripts/modelos/\*. js: define os modelos.</span><span class="sxs-lookup"><span data-stu-id="7a779-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="7a779-144">Aplicativo/scripts/exibições/\*. js: define as exibições.</span><span class="sxs-lookup"><span data-stu-id="7a779-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="7a779-145">Aplicativo/scripts/controladores/\*. js: define os controladores.</span><span class="sxs-lookup"><span data-stu-id="7a779-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="7a779-146">Aplicativo/scripts/rotas, Scripts/app/router.js: Define as rotas.</span><span class="sxs-lookup"><span data-stu-id="7a779-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="7a779-147">Modelos /\*.hbs: define os modelos Guidões.</span><span class="sxs-lookup"><span data-stu-id="7a779-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="7a779-148">Vamos examinar alguns desses scripts em mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="7a779-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="7a779-149">Modelos</span><span class="sxs-lookup"><span data-stu-id="7a779-149">Models</span></span>

<span data-ttu-id="7a779-150">Os modelos são definidos na pasta Scripts/aplicativo/modelos.</span><span class="sxs-lookup"><span data-stu-id="7a779-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="7a779-151">Há dois arquivos de modelo: todoItem.js e todoList.js.</span><span class="sxs-lookup"><span data-stu-id="7a779-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="7a779-152">**todo.Model.js** define os modelos de cliente (navegador) para as listas de tarefas pendentes.</span><span class="sxs-lookup"><span data-stu-id="7a779-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="7a779-153">Há duas classes de modelo: todoItem e lista de tarefas.</span><span class="sxs-lookup"><span data-stu-id="7a779-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="7a779-154">Em Ember, os modelos são subclasses de DS. Modelo.</span><span class="sxs-lookup"><span data-stu-id="7a779-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="7a779-155">Um modelo pode ter propriedades com atributos:</span><span class="sxs-lookup"><span data-stu-id="7a779-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="7a779-156">Modelos podem definir relações com outros modelos:</span><span class="sxs-lookup"><span data-stu-id="7a779-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="7a779-157">Modelos podem ter computada propriedades vincular a outras propriedades:</span><span class="sxs-lookup"><span data-stu-id="7a779-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="7a779-158">Modelos podem ter funções do observador, que são invocadas quando uma propriedade observada alterações:</span><span class="sxs-lookup"><span data-stu-id="7a779-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="7a779-159">Exibições</span><span class="sxs-lookup"><span data-stu-id="7a779-159">Views</span></span>

<span data-ttu-id="7a779-160">As exibições são definidas na pasta Scripts/aplicativo/modos de exibição.</span><span class="sxs-lookup"><span data-stu-id="7a779-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="7a779-161">Um modo de exibição converte os eventos do aplicativo da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="7a779-161">A view translates events from the application UI.</span></span> <span data-ttu-id="7a779-162">Um manipulador de eventos pode retornar chamada para funções de controlador ou simplesmente chamar o contexto de dados diretamente.</span><span class="sxs-lookup"><span data-stu-id="7a779-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="7a779-163">Por exemplo, o código a seguir é de views/TodoItemEditView.js.</span><span class="sxs-lookup"><span data-stu-id="7a779-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="7a779-164">Ele define o manipulação de eventos para um campo de texto de entrada.</span><span class="sxs-lookup"><span data-stu-id="7a779-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="7a779-165">Controlador</span><span class="sxs-lookup"><span data-stu-id="7a779-165">Controller</span></span>

<span data-ttu-id="7a779-166">Os controladores são definidos na pasta Scripts/aplicativo/controladores.</span><span class="sxs-lookup"><span data-stu-id="7a779-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="7a779-167">Para representar um único modelo, estender `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="7a779-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="7a779-168">Um controlador também pode representar uma coleção de modelos, estendendo `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="7a779-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="7a779-169">Por exemplo, o TodoListController representa uma matriz de `todoList` objetos.</span><span class="sxs-lookup"><span data-stu-id="7a779-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="7a779-170">O controlador classifica por ID de lista de tarefas, em ordem decrescente:</span><span class="sxs-lookup"><span data-stu-id="7a779-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="7a779-171">O controlador define uma função chamada `addTodoList`, que cria uma nova lista de tarefas e o adiciona à matriz.</span><span class="sxs-lookup"><span data-stu-id="7a779-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="7a779-172">Para ver como essa função é chamada, abra o arquivo de modelo chamado todoListTemplate.html na pasta modelos.</span><span class="sxs-lookup"><span data-stu-id="7a779-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="7a779-173">O código de modelo a seguir associa um botão para o `addTodoList` função:</span><span class="sxs-lookup"><span data-stu-id="7a779-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="7a779-174">O controlador também contém um `error` propriedade, que contém uma mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="7a779-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="7a779-175">Aqui está o código de modelo para exibir a mensagem de erro (também em todoListTemplate.html):</span><span class="sxs-lookup"><span data-stu-id="7a779-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="7a779-176">Rotas</span><span class="sxs-lookup"><span data-stu-id="7a779-176">Routes</span></span>

<span data-ttu-id="7a779-177">Router.js define as rotas e o modelo padrão para exibir conjuntos de backup do estado do aplicativo e URLs de correspondências para rotas:</span><span class="sxs-lookup"><span data-stu-id="7a779-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="7a779-178">TodoListRoute.js carrega dados para o TodoListRoute, substituindo a função setupController:</span><span class="sxs-lookup"><span data-stu-id="7a779-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="7a779-179">Ember usa convenções de nomenclatura para corresponder URLs, nomes de rotas, controladores e modelos.</span><span class="sxs-lookup"><span data-stu-id="7a779-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="7a779-180">Para obter mais informações, consulte [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) a documentação de EmberJS.</span><span class="sxs-lookup"><span data-stu-id="7a779-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="7a779-181">Modelos</span><span class="sxs-lookup"><span data-stu-id="7a779-181">Templates</span></span>

<span data-ttu-id="7a779-182">A pasta de modelos contém quatro modelos:</span><span class="sxs-lookup"><span data-stu-id="7a779-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="7a779-183">Application.HBS: O modelo padrão que é renderizado quando o aplicativo é iniciado.</span><span class="sxs-lookup"><span data-stu-id="7a779-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="7a779-184">About.HBS: O modelo da rota "/ sobre".</span><span class="sxs-lookup"><span data-stu-id="7a779-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="7a779-185">index.HBS: O modelo para a raiz de rota "/".</span><span class="sxs-lookup"><span data-stu-id="7a779-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="7a779-186">todoList.hbs: O modelo para o "/ todo" rota.</span><span class="sxs-lookup"><span data-stu-id="7a779-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="7a779-187">\_NavBar.HBS: O modelo define o menu de navegação.</span><span class="sxs-lookup"><span data-stu-id="7a779-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="7a779-188">O modelo de aplicativo age como uma página mestra.</span><span class="sxs-lookup"><span data-stu-id="7a779-188">The application template acts like a master page.</span></span> <span data-ttu-id="7a779-189">Ele contém um cabeçalho, um rodapé e uma "{{tomada}}" para inserir os outros modelos dependendo da rota.</span><span class="sxs-lookup"><span data-stu-id="7a779-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="7a779-190">Para obter mais informações sobre modelos de aplicativos em Ember, consulte [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="7a779-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="7a779-191">O "/ todoList" modelo contém duas expressões de loop.</span><span class="sxs-lookup"><span data-stu-id="7a779-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="7a779-192">O loop externo é `{{#each controller}}`e o loop é `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="7a779-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="7a779-193">O código a seguir mostra um interno `Ember.Checkbox` exibir um personalizado `App.TodoItemEditView`e um link com um `deleteTodo` ação.</span><span class="sxs-lookup"><span data-stu-id="7a779-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="7a779-194">O `HtmlHelperExtensions` classe, definida no Controllers/HtmlHelperExensions.cs, define um auxiliar de função em cache e inserir modelo arquivos ao **depurar** é definido como **true** no arquivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="7a779-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="7a779-195">Essa função é chamada do arquivo de exibição do ASP.NET MVC definido na Views/Home/App.cshtml:</span><span class="sxs-lookup"><span data-stu-id="7a779-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="7a779-196">A função chamado sem argumentos, processa todos os arquivos de modelo na pasta modelos.</span><span class="sxs-lookup"><span data-stu-id="7a779-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="7a779-197">Você também pode especificar uma subpasta ou um arquivo de modelo específico.</span><span class="sxs-lookup"><span data-stu-id="7a779-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="7a779-198">Quando **depurar** é **false** em Web. config, o aplicativo inclui o item de pacote "~/bundles/templates".</span><span class="sxs-lookup"><span data-stu-id="7a779-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="7a779-199">Este item de pacote é adicionado no BundleConfig.cs, usando a biblioteca de compilador Guidões:</span><span class="sxs-lookup"><span data-stu-id="7a779-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]

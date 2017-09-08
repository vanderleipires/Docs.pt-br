---
title: "Estrutura Knockout. js MVVM no núcleo do ASP.NET"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b20e3b23-1c51-47bf-adac-91b5048567e0
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/knockout
ms.openlocfilehash: 87b4fdc86f6bb870ae0a8cc85688a549fd0740ac
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a><span data-ttu-id="d5296-103">Estrutura Knockout. js MVVM no núcleo do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d5296-103">Knockout.js MVVM Framework in ASP.NET Core</span></span>

<span data-ttu-id="d5296-104">Por [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="d5296-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="d5296-105">Separação é uma biblioteca JavaScript popular que simplifica a criação de interfaces de usuário complexas com base em dados.</span><span class="sxs-lookup"><span data-stu-id="d5296-105">Knockout is a popular JavaScript library that simplifies the creation of complex data-based user interfaces.</span></span> <span data-ttu-id="d5296-106">Ele pode ser usado sozinho ou com outras bibliotecas, como jQuery.</span><span class="sxs-lookup"><span data-stu-id="d5296-106">It can be used alone or with other libraries, such as jQuery.</span></span> <span data-ttu-id="d5296-107">Sua finalidade principal é associar os elementos de interface do usuário para um modelo de dados definido como um objeto de JavaScript, de modo que quando as alterações são feitas para a interface do usuário, o modelo é atualizado e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="d5296-107">Its primary purpose is to bind UI elements to an underlying data model defined as a JavaScript object, such that when changes are made to the UI, the model is updated, and vice versa.</span></span> <span data-ttu-id="d5296-108">Knockout facilita o uso de um padrão Model-View-ViewModel (MVVM) no comportamento do cliente de um aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="d5296-108">Knockout facilitates the use of a Model-View-ViewModel (MVVM) pattern in a web application's client-side behavior.</span></span> <span data-ttu-id="d5296-109">Os dois principais conceitos um deve saber ao trabalhar com a implementação do MVVM do Knockout são observáveis e associações.</span><span class="sxs-lookup"><span data-stu-id="d5296-109">The two main concepts one must learn when working with Knockout's MVVM implementation are Observables and Bindings.</span></span>

## <a name="getting-started"></a><span data-ttu-id="d5296-110">Introdução</span><span class="sxs-lookup"><span data-stu-id="d5296-110">Getting started</span></span>

<span data-ttu-id="d5296-111">Separação é implantada como um único arquivo de JavaScript, para instalar e usá-lo é muito simples usando [bower](bower.md).</span><span class="sxs-lookup"><span data-stu-id="d5296-111">Knockout is deployed as a single JavaScript file, so installing and using it is very straightforward using [bower](bower.md).</span></span> <span data-ttu-id="d5296-112">Supondo que você já tiver [bower](bower.md) e [gulp](using-gulp.md) configurado, abra *bower. JSON* no seu ASP.NET Core do projeto e adicionar a dependência de separação, conforme mostrado aqui:</span><span class="sxs-lookup"><span data-stu-id="d5296-112">Assuming you already have [bower](bower.md) and [gulp](using-gulp.md) configured, open *bower.json* in your ASP.NET Core project and add the knockout dependency as shown here:</span></span>

```json
{
  "name": "KnockoutDemo",
  "private": true,
  "dependencies": {
    "knockout" : "^3.3.0"
  },
  "exportsOverride": {
  }
}
```

<span data-ttu-id="d5296-113">Com isso em vigor, você pode manualmente executar bower abrindo o Explorador do Executador de tarefas (na exibição ‣ outras janelas ‣ Explorador do Executador de tarefas) e, em seguida, em tarefas, clique em bower e selecione Executar.</span><span class="sxs-lookup"><span data-stu-id="d5296-113">With this in place, you can then manually run bower by opening the Task Runner Explorer (under View ‣ Other Windows ‣ Task Runner Explorer) and then under Tasks, right-click on bower and select Run.</span></span> <span data-ttu-id="d5296-114">O resultado deve ser semelhante a este:</span><span class="sxs-lookup"><span data-stu-id="d5296-114">The result should appear similar to this:</span></span>

![bower knockout em execução no Explorador do Executador de tarefas](knockout/_static/bower-knockout.png)

<span data-ttu-id="d5296-116">Agora, se você olhar no seu projeto `wwwroot` pasta, você deve ver knockout instalado na pasta lib.</span><span class="sxs-lookup"><span data-stu-id="d5296-116">Now if you look in your project's `wwwroot` folder, you should see knockout installed under the lib folder.</span></span>

![Knockout instalado na pasta lib](knockout/_static/wwwroot-knockout.png)

<span data-ttu-id="d5296-118">Recomenda-se em seu ambiente de produção referência knockout por meio de uma rede de fornecimento de conteúdo ou CDN, pois isso aumenta a probabilidade de que os usuários já terá uma cópia armazenada em cache do arquivo e, portanto, não serão necessário baixá-lo em todos os.</span><span class="sxs-lookup"><span data-stu-id="d5296-118">It's recommended that in your production environment you reference knockout via a Content Delivery Network, or CDN, as this increases the likelihood that your users will already have a cached copy of the file and thus will not need to download it at all.</span></span> <span data-ttu-id="d5296-119">Knockout está disponível em vários CDNs, incluindo a Microsoft Ajax CDN, aqui:</span><span class="sxs-lookup"><span data-stu-id="d5296-119">Knockout is available on several CDNs, including the Microsoft Ajax CDN, here:</span></span>

[<span data-ttu-id="d5296-120">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="d5296-120">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

<span data-ttu-id="d5296-121">Para incluir Knockout em uma página que irá usá-la, basta adicionar uma `<script>` elemento para referenciar o arquivo de onde você hospedará ele (com o seu aplicativo, ou por meio de um CDN):</span><span class="sxs-lookup"><span data-stu-id="d5296-121">To include Knockout on a page that will use it, simply add a `<script>` element referencing the file from wherever you will be hosting it (with your application, or via a CDN):</span></span>

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a><span data-ttu-id="d5296-122">Observáveis ViewModels e associação simples</span><span class="sxs-lookup"><span data-stu-id="d5296-122">Observables, ViewModels, and simple binding</span></span>

<span data-ttu-id="d5296-123">Você já pode estar familiarizado com o uso de JavaScript para manipular elementos em uma página da web, via acesso direto ao DOM ou usando uma biblioteca como jQuery.</span><span class="sxs-lookup"><span data-stu-id="d5296-123">You may already be familiar with using JavaScript to manipulate elements on a web page, either via direct access to the DOM or using a library like jQuery.</span></span> <span data-ttu-id="d5296-124">Normalmente, esse tipo de comportamento é obtido escrevendo código para definir valores de elemento diretamente em resposta a determinadas ações do usuário.</span><span class="sxs-lookup"><span data-stu-id="d5296-124">Typically this kind of behavior is achieved by writing code to directly set element values in response to certain user actions.</span></span> <span data-ttu-id="d5296-125">Com Knockout, uma abordagem declarativa é obtida em vez disso, por meio do qual os elementos na página estão associados a propriedades em um objeto.</span><span class="sxs-lookup"><span data-stu-id="d5296-125">With Knockout, a declarative approach is taken instead, through which elements on the page are bound to properties on an object.</span></span> <span data-ttu-id="d5296-126">Em vez de escrever código para manipular elementos DOM, ações do usuário simplesmente interagem com o objeto ViewModel e Knockout cuida de garantir que os elementos da página são sincronizados.</span><span class="sxs-lookup"><span data-stu-id="d5296-126">Instead of writing code to manipulate DOM elements, user actions simply interact with the ViewModel object, and Knockout takes care of ensuring the page elements are synchronized.</span></span>

<span data-ttu-id="d5296-127">Como um exemplo simples, considere a lista de página abaixo.</span><span class="sxs-lookup"><span data-stu-id="d5296-127">As a simple example, consider the page list below.</span></span> <span data-ttu-id="d5296-128">Ele inclui um `<span>` elemento com um `data-bind` indicando que o conteúdo de texto deve ser associado aos authorName de atributo.</span><span class="sxs-lookup"><span data-stu-id="d5296-128">It includes a `<span>` element with a `data-bind` attribute indicating that the text content should be bound to authorName.</span></span> <span data-ttu-id="d5296-129">Em seguida, em um bloco JavaScript viewModel um variável é definido com uma única propriedade, `authorName`, um conjunto de valores.</span><span class="sxs-lookup"><span data-stu-id="d5296-129">Next, in a JavaScript block a variable viewModel is defined with a single property, `authorName`, set to some value.</span></span> <span data-ttu-id="d5296-130">Por fim, uma chamada para `ko.applyBindings` é feita, passando essa variável viewModel.</span><span class="sxs-lookup"><span data-stu-id="d5296-130">Finally, a call to `ko.applyBindings` is made, passing in this viewModel variable.</span></span>

```html
<html>
<head>
    <script type="text/javascript" src="lib/knockout/knockout.js"></script>
</head>
<body>
    <h1>Some Article</h1>
    <p>
        By <span data-bind="text: authorName"></span>
    </p>
    <script type="text/javascript">
      var viewModel = {
        authorName: 'Steve Smith'
      };
      ko.applyBindings(viewModel);
    </script>
</body>
</html>
```

<span data-ttu-id="d5296-131">Quando exibido no navegador, o conteúdo de <span> elemento é substituído pelo valor na variável viewModel:</span><span class="sxs-lookup"><span data-stu-id="d5296-131">When viewed in the browser, the content of the <span> element is replaced with the value in the viewModel variable:</span></span>

![associação simples de separação](knockout/_static/simple-binding-screenshot.png)

<span data-ttu-id="d5296-133">Agora temos trabalho simples associação unidirecional.</span><span class="sxs-lookup"><span data-stu-id="d5296-133">We now have simple one-way binding working.</span></span> <span data-ttu-id="d5296-134">Observe que isso no código que escrevemos JavaScript para atribuir um valor para o conteúdo do alcance.</span><span class="sxs-lookup"><span data-stu-id="d5296-134">Notice that nowhere in the code did we write JavaScript to assign a value to the span's contents.</span></span> <span data-ttu-id="d5296-135">Se quisermos manipular o ViewModel, podemos ir um pouco mais e adicionar uma caixa de texto de entrada HTML e associar ao seu valor, como para:</span><span class="sxs-lookup"><span data-stu-id="d5296-135">If we want to manipulate the ViewModel, we can take this a step further and add an HTML input textbox, and bind to its value, like so:</span></span>

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

<span data-ttu-id="d5296-136">Recarregar a página, podemos ver que esse valor, na verdade, está associado à caixa de entrada:</span><span class="sxs-lookup"><span data-stu-id="d5296-136">Reloading the page, we see that this value is indeed bound to the input box:</span></span>

![associação de entrada de separação](knockout/_static/input-binding-screenshot.png)

<span data-ttu-id="d5296-138">No entanto, se alterarmos o valor na caixa de texto, o valor correspondente no `<span>` elemento não é alterado.</span><span class="sxs-lookup"><span data-stu-id="d5296-138">However, if we change the value in the textbox, the corresponding value in the `<span>` element doesn't change.</span></span> <span data-ttu-id="d5296-139">Por quê?</span><span class="sxs-lookup"><span data-stu-id="d5296-139">Why not?</span></span>

<span data-ttu-id="d5296-140">O problema é que nada notificadas de `<span>` que ele precisa ser atualizado.</span><span class="sxs-lookup"><span data-stu-id="d5296-140">The issue is that nothing notified the `<span>` that it needed to be updated.</span></span> <span data-ttu-id="d5296-141">Simplesmente atualizar ViewModel não sozinho suficientes, a menos que as propriedades do ViewModel são encapsuladas em um tipo especial.</span><span class="sxs-lookup"><span data-stu-id="d5296-141">Simply updating the ViewModel isn't by itself sufficient, unless the ViewModel's properties are wrapped in a special type.</span></span> <span data-ttu-id="d5296-142">Precisamos usar **observables** no ViewModel para qualquer propriedade que precisam ter alterações atualizadas automaticamente à medida que eles ocorrem.</span><span class="sxs-lookup"><span data-stu-id="d5296-142">We need to use **observables** in the ViewModel for any properties that need to have changes automatically updated as they occur.</span></span> <span data-ttu-id="d5296-143">Alterando o ViewModel usar `ko.observable("value")` em vez de apenas "valor", o ViewModel atualizará todos os elementos HTML que estão associados a seu valor sempre que ocorre uma alteração.</span><span class="sxs-lookup"><span data-stu-id="d5296-143">By changing the ViewModel to use `ko.observable("value")` instead of just "value", the ViewModel will update any HTML elements that are bound to its value whenever a change occurs.</span></span> <span data-ttu-id="d5296-144">Observe que as caixas de entrada não atualizar seu valor até que eles perdem foco, você não verá alterações associado elementos conforme você digita.</span><span class="sxs-lookup"><span data-stu-id="d5296-144">Note that input boxes don't update their value until they lose focus, so you won't see changes to bound elements as you type.</span></span>

> [!NOTE]
> <span data-ttu-id="d5296-145">Adicionando suporte para a atualização dinâmica após cada keypress é simplesmente uma questão de adicionar `valueUpdate: "afterkeydown"` para o `data-bind` conteúdo do atributo.</span><span class="sxs-lookup"><span data-stu-id="d5296-145">Adding support for live updating after each keypress is simply a matter of adding `valueUpdate: "afterkeydown"` to the `data-bind` attribute's contents.</span></span> <span data-ttu-id="d5296-146">Você também pode obter esse comportamento usando `data-bind="textInput: authorName"` para obter atualizações de instantâneas de valores.</span><span class="sxs-lookup"><span data-stu-id="d5296-146">You can also get this behavior by using `data-bind="textInput: authorName"` to get instant updates of values.</span></span> 

<span data-ttu-id="d5296-147">Nosso viewModel, após a atualização para usar ko.observable:</span><span class="sxs-lookup"><span data-stu-id="d5296-147">Our viewModel, after updating it to use ko.observable:</span></span>

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="d5296-148">Separação dá suporte a vários tipos diferentes de associações.</span><span class="sxs-lookup"><span data-stu-id="d5296-148">Knockout supports a number of different kinds of bindings.</span></span> <span data-ttu-id="d5296-149">Até agora, vimos como vincular a `text` e `value`.</span><span class="sxs-lookup"><span data-stu-id="d5296-149">So far we've seen how to bind to `text` and to `value`.</span></span> <span data-ttu-id="d5296-150">Você também pode associar a qualquer determinado atributo.</span><span class="sxs-lookup"><span data-stu-id="d5296-150">You can also bind to any given attribute.</span></span> <span data-ttu-id="d5296-151">Por exemplo, para criar um hiperlink com uma marca de âncora de `src` atributo pode ser associado ao viewModel.</span><span class="sxs-lookup"><span data-stu-id="d5296-151">For instance, to create a hyperlink with an anchor tag, the `src` attribute can be bound to the viewModel.</span></span> <span data-ttu-id="d5296-152">Knockout também dá suporte à associação a funções.</span><span class="sxs-lookup"><span data-stu-id="d5296-152">Knockout also supports binding to functions.</span></span> <span data-ttu-id="d5296-153">Para demonstrar isso, vamos atualizar viewModel para incluir o identificador do twitter do autor e exibir o identificador do twitter como um link para a página do twitter do autor.</span><span class="sxs-lookup"><span data-stu-id="d5296-153">To demonstrate this, let's update the viewModel to include the author's twitter handle, and display the twitter handle as a link to the author's twitter page.</span></span> <span data-ttu-id="d5296-154">Faremos isso em três etapas.</span><span class="sxs-lookup"><span data-stu-id="d5296-154">We'll do this in three stages.</span></span>

<span data-ttu-id="d5296-155">Primeiro, adicione o HTML para exibir o hiperlink, o que vamos mostrar entre parênteses após o nome do autor:</span><span class="sxs-lookup"><span data-stu-id="d5296-155">First, add the HTML to display the hyperlink, which we'll show in parentheses after the author's name:</span></span>

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

<span data-ttu-id="d5296-156">Em seguida, atualize o viewModel para incluir as propriedades twitterUrl e twitterAlias:</span><span class="sxs-lookup"><span data-stu-id="d5296-156">Next, update the viewModel to include the twitterUrl and twitterAlias properties:</span></span>

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith'),
  twitterAlias: ko.observable('@ardalis'),
  twitterUrl: ko.computed(function() {
    return "https://twitter.com/";
  }, this)
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="d5296-157">Observe que agora é ainda não tiver atualizado twitterUrl para ir para a URL correta para este alias twitter – ele é simplesmente apontando twitter.com.</span><span class="sxs-lookup"><span data-stu-id="d5296-157">Notice that at this point we haven't yet updated the twitterUrl to go to the correct URL for this twitter alias – it's just pointing at twitter.com.</span></span> <span data-ttu-id="d5296-158">Além disso, observe que estamos usando uma nova função de separação, `computed`, para twitterUrl.</span><span class="sxs-lookup"><span data-stu-id="d5296-158">Also notice that we're using a new Knockout function, `computed`, for twitterUrl.</span></span> <span data-ttu-id="d5296-159">Essa é uma função observável que notificará quaisquer elementos de interface do usuário se ele for alterado.</span><span class="sxs-lookup"><span data-stu-id="d5296-159">This is an observable function that will notify any UI elements if it changes.</span></span> <span data-ttu-id="d5296-160">No entanto, para que ele tem acesso a outras propriedades no viewModel, precisamos alterar como estamos criando viewModel, de forma que cada propriedade é sua própria instrução.</span><span class="sxs-lookup"><span data-stu-id="d5296-160">However, for it to have access to other properties in the viewModel, we need to change how we are creating the viewModel, so that each property is its own statement.</span></span>

<span data-ttu-id="d5296-161">A declaração de viewModel revisado é mostrada abaixo.</span><span class="sxs-lookup"><span data-stu-id="d5296-161">The revised viewModel declaration is shown below.</span></span> <span data-ttu-id="d5296-162">Agora é declarada como uma função.</span><span class="sxs-lookup"><span data-stu-id="d5296-162">It is now declared as a function.</span></span> <span data-ttu-id="d5296-163">Observe que cada propriedade é sua própria instrução agora, terminando com um ponto e vírgula.</span><span class="sxs-lookup"><span data-stu-id="d5296-163">Notice that each property is its own statement now, ending with a semicolon.</span></span> <span data-ttu-id="d5296-164">Além disso, observe que, para acessar o valor da propriedade twitterAlias, é necessário executá-lo, para que sua referência inclui ().</span><span class="sxs-lookup"><span data-stu-id="d5296-164">Also notice that to access the twitterAlias property value, we need to execute it, so its reference includes ().</span></span>

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this)
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="d5296-165">O resultado funciona conforme esperado no navegador:</span><span class="sxs-lookup"><span data-stu-id="d5296-165">The result works as expected in the browser:</span></span>

![hiperlink de separação](knockout/_static/hyperlink-screenshot.png)

<span data-ttu-id="d5296-167">Knockout também dá suporte à associação a determinados eventos do elemento da interface do usuário, como o evento de clique.</span><span class="sxs-lookup"><span data-stu-id="d5296-167">Knockout also supports binding to certain UI element events, such as the click event.</span></span> <span data-ttu-id="d5296-168">Isso permite que você facilmente e declarativamente vincular elementos de interface do usuário a funções dentro de viewModel do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d5296-168">This allows you to easily and declaratively bind UI elements to functions within the application's viewModel.</span></span> <span data-ttu-id="d5296-169">Como um exemplo simples, podemos adicionar um botão que, quando clicado, modifica twitterAlias do autor para ser todas em maiusculas.</span><span class="sxs-lookup"><span data-stu-id="d5296-169">As a simple example, we can add a button that, when clicked, modifies the author's twitterAlias to be all caps.</span></span>

<span data-ttu-id="d5296-170">Primeiro, podemos adicionar o botão, associação para o botão em eventos e referenciando o nome da função, vamos adicionar ao viewModel:</span><span class="sxs-lookup"><span data-stu-id="d5296-170">First, we add the button, binding to the button's click event, and referencing the function name we're going to add to the viewModel:</span></span>

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

<span data-ttu-id="d5296-171">Em seguida, adicione a função ao viewModel e conectá-lo para modificar o estado do viewModel.</span><span class="sxs-lookup"><span data-stu-id="d5296-171">Then, add the function to the viewModel, and wire it up to modify the viewModel's state.</span></span> <span data-ttu-id="d5296-172">Observe que para definir um novo valor para a propriedade twitterAlias, podemos chamá-lo como um método e passar o novo valor.</span><span class="sxs-lookup"><span data-stu-id="d5296-172">Notice that to set a new value to the twitterAlias property, we call it as a method and pass in the new value.</span></span>

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this);

  this.capitalizeTwitterAlias = function() {
    var currentValue = this.twitterAlias();
    this.twitterAlias(currentValue.toUpperCase());
  }
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="d5296-173">A execução do código e clicando no botão modifica o link exibido conforme o esperado:</span><span class="sxs-lookup"><span data-stu-id="d5296-173">Running the code and clicking the button modifies the displayed link as expected:</span></span>

![colocar em maiuscula de hiperlink](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a><span data-ttu-id="d5296-175">Fluxo de controle</span><span class="sxs-lookup"><span data-stu-id="d5296-175">Control flow</span></span>

<span data-ttu-id="d5296-176">Knockout inclui associações que podem executar operações condicionais e loop.</span><span class="sxs-lookup"><span data-stu-id="d5296-176">Knockout includes bindings that can perform conditional and looping operations.</span></span> <span data-ttu-id="d5296-177">Operações loop são especialmente úteis para listas de dados de associação a listas, menus e grades ou tabelas de interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="d5296-177">Looping operations are especially useful for binding lists of data to UI lists, menus, and grids or tables.</span></span> <span data-ttu-id="d5296-178">A associação de foreach irá iterar em uma matriz.</span><span class="sxs-lookup"><span data-stu-id="d5296-178">The foreach binding will iterate over an array.</span></span> <span data-ttu-id="d5296-179">Quando usado com uma matriz observável, ele atualizará automaticamente os elementos de interface do usuário quando itens são adicionados ou removidos da matriz, sem recriar todos os elementos na árvore de interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="d5296-179">When used with an observable array, it will automatically update the UI elements when items are added or removed from the array, without re-creating every element in the UI tree.</span></span> <span data-ttu-id="d5296-180">O exemplo a seguir usa um novo viewModel que inclui uma matriz observável dos resultados de jogos.</span><span class="sxs-lookup"><span data-stu-id="d5296-180">The following example uses a new viewModel which includes an observable array of game results.</span></span> <span data-ttu-id="d5296-181">Ele é associado a uma tabela simples com duas colunas usando um `foreach` associação no `<tbody>` elemento.</span><span class="sxs-lookup"><span data-stu-id="d5296-181">It is bound to a simple table with two columns using a `foreach` binding on the `<tbody>` element.</span></span> <span data-ttu-id="d5296-182">Cada `<tr>` elemento dentro do `<tbody>` será associada a um elemento da coleção gameResults.</span><span class="sxs-lookup"><span data-stu-id="d5296-182">Each `<tr>` element within `<tbody>` will be bound to an element of the gameResults collection.</span></span>

```html
<h1>Record</h1>
<table>
    <thead>
        <tr>
            <th>Opponent</th>
            <th>Result</th>
        </tr>
    </thead>
    <tbody data-bind="foreach: gameResults">
        <tr>
            <td data-bind="text:opponent"></td>
            <td data-bind="text:result"></td>
        </tr>
    </tbody>
</table>
<script type="text/javascript">
  function GameResult(opponent, result) {
    var self = this;
    self.opponent = opponent;
    self.result = ko.observable(result);
  }

  function ViewModel() {
    var self = this;

    self.resultChoices = ["Win", "Loss", "Tie"];

    self.gameResults = ko.observableArray([
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Michelle", self.resultChoices[1])
    ]);
  };
  ko.applyBindings(new ViewModel);
</script>
```

<span data-ttu-id="d5296-183">Observe que neste momento estamos usando ViewModel com uma letra maiuscula "V" porque esperamos para construí-lo usando "nova" (na chamada applyBindings).</span><span class="sxs-lookup"><span data-stu-id="d5296-183">Notice that this time we're using ViewModel with a capital “V" because we expect to construct it using “new" (in the applyBindings call).</span></span> <span data-ttu-id="d5296-184">Quando executada, a página resulta na seguinte saída:</span><span class="sxs-lookup"><span data-stu-id="d5296-184">When executed, the page results in the following output:</span></span>

![modelo de exibição de registro de separação](knockout/_static/record-screenshot.png)

<span data-ttu-id="d5296-186">Para demonstrar que a coleção observável está funcionando, vamos adicionar mais funcionalidade.</span><span class="sxs-lookup"><span data-stu-id="d5296-186">To demonstrate that the observable collection is working, let's add a bit more functionality.</span></span> <span data-ttu-id="d5296-187">Podemos incluem a capacidade de gravar os resultados de outro jogo ao ViewModel e, em seguida, adicione um botão e algumas interfaces do usuário para trabalhar com essa nova função.</span><span class="sxs-lookup"><span data-stu-id="d5296-187">We can include the ability to record the results of another game to the ViewModel, and then add a button and some UI to work with this new function.</span></span>  <span data-ttu-id="d5296-188">Primeiro, vamos criar o método addResult:</span><span class="sxs-lookup"><span data-stu-id="d5296-188">First, let's create the addResult method:</span></span>

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

<span data-ttu-id="d5296-189">Associar a este método em um botão usando o `click` associação:</span><span class="sxs-lookup"><span data-stu-id="d5296-189">Bind this method to a button using the `click` binding:</span></span>

```html
<button data-bind="click: addResult">Add New Result</button>
```

<span data-ttu-id="d5296-190">Abra a página no navegador e clique no botão algumas vezes, resultando em uma nova linha na tabela com cada clique:</span><span class="sxs-lookup"><span data-stu-id="d5296-190">Open the page in the browser and click the button a couple of times, resulting in a new table row with each click:</span></span>

![Adicionar os resultados](knockout/_static/record-addresult-screenshot.png)

<span data-ttu-id="d5296-192">Há algumas maneiras de suporte à adição de novos registros na interface de usuário, normalmente em linha ou de forma separada.</span><span class="sxs-lookup"><span data-stu-id="d5296-192">There are a few ways to support adding new records in the UI, typically either inline or in a separate form.</span></span> <span data-ttu-id="d5296-193">Podemos facilmente modificar a tabela para usar as caixas de texto e dropdownlists para que tudo é editável.</span><span class="sxs-lookup"><span data-stu-id="d5296-193">We can easily modify the table to use textboxes and dropdownlists so that the whole thing is editable.</span></span> <span data-ttu-id="d5296-194">Alterar o `<tr>` elemento conforme mostrado:</span><span class="sxs-lookup"><span data-stu-id="d5296-194">Just change the `<tr>` element as shown:</span></span>

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

<span data-ttu-id="d5296-195">Observe que `$root` refere-se à raiz ViewModel, que é onde as opções possíveis são expostas.</span><span class="sxs-lookup"><span data-stu-id="d5296-195">Note that `$root` refers to the root ViewModel, which is where the possible choices are exposed.</span></span> <span data-ttu-id="d5296-196">`$data`refere-se de que o modelo atual está dentro de um determinado contexto - nesse caso, que ele se refere a um elemento individual da matriz resultChoices, cada um deles é uma cadeia de caracteres simple.</span><span class="sxs-lookup"><span data-stu-id="d5296-196">`$data` refers to whatever the current model is within a given context - in this case it refers to an individual element of the resultChoices array, each of which is a simple string.</span></span>

<span data-ttu-id="d5296-197">Com essa alteração, toda a grade fica editável:</span><span class="sxs-lookup"><span data-stu-id="d5296-197">With this change, the entire grid becomes editable:</span></span>

![Grade editável](knockout/_static/editable-grid-screenshot.png)

<span data-ttu-id="d5296-199">Se não estivéssemos usando Knockout, podemos poderia obter tudo isso usando jQuery, mas provavelmente não seria ser quase tão eficiente.</span><span class="sxs-lookup"><span data-stu-id="d5296-199">If we weren't using Knockout, we could achieve all of this using jQuery, but most likely it would not be nearly as efficient.</span></span> <span data-ttu-id="d5296-200">Knockout controla quais dados associados a itens no ViewModel correspondem quais elementos de interface do usuário e atualiza somente os elementos que precisam ser adicionados, removidos ou atualizados.</span><span class="sxs-lookup"><span data-stu-id="d5296-200">Knockout tracks which bound data items in the ViewModel correspond to which UI elements, and only updates those elements that need to be added, removed, or updated.</span></span> <span data-ttu-id="d5296-201">Seria necessário esforço significativo para fazer isso nos usando jQuery ou manipulação direta do DOM, e mesmo se quiséssemos, em seguida exibir os resultados de agregação (como um registro de ganho perda) com base nos dados da tabela, é preciso percorrer a ele mais uma vez e analisar o Elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="d5296-201">It would take significant effort to achieve this ourselves using jQuery or direct DOM manipulation, and even then if we then wanted to display aggregate results (such as a win-loss record) based on the table's data, we would need to once more loop through it and parse the HTML elements.</span></span>  <span data-ttu-id="d5296-202">Com Knockout, exibir o registro de ganho perda é simples.</span><span class="sxs-lookup"><span data-stu-id="d5296-202">With Knockout, displaying the win-loss record is trivial.</span></span> <span data-ttu-id="d5296-203">Podemos executar cálculos em ViewModel em si e, em seguida, exibi-lo com uma associação de texto simples e um `<span>`.</span><span class="sxs-lookup"><span data-stu-id="d5296-203">We can perform the calculations within the ViewModel itself, and then display it with a simple text binding and a `<span>`.</span></span>

<span data-ttu-id="d5296-204">Para criar a cadeia de caracteres do registro de ganho perda, podemos usar um observável computada.</span><span class="sxs-lookup"><span data-stu-id="d5296-204">To build the win-loss record string, we can use a computed observable.</span></span> <span data-ttu-id="d5296-205">Observe que faz referência às propriedades observáveis nos ViewModel deve ser chamadas de função, caso contrário, eles não recuperará o valor do observável (ou seja, `gameResults()` não `gameResults` no código mostrado):</span><span class="sxs-lookup"><span data-stu-id="d5296-205">Note that references to observable properties within the ViewModel must be function calls, otherwise they will not retrieve the value of the observable (i.e. `gameResults()` not `gameResults` in the code shown):</span></span>

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

<span data-ttu-id="d5296-206">Associar a essa função para um intervalo dentro do `<h1>` elemento na parte superior da página:</span><span class="sxs-lookup"><span data-stu-id="d5296-206">Bind this function to a span within the `<h1>` element at the top of the page:</span></span>

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

<span data-ttu-id="d5296-207">O resultado:</span><span class="sxs-lookup"><span data-stu-id="d5296-207">The result:</span></span>

![Ganhos e perdas](knockout/_static/record-winloss-screenshot.png)

<span data-ttu-id="d5296-209">Adicionando linhas ou modificando o elemento selecionado na coluna de resultado de qualquer linha atualizará o registro mostrado na parte superior da janela.</span><span class="sxs-lookup"><span data-stu-id="d5296-209">Adding rows or modifying the selected element in any row's Result column will update the record shown at the top of the window.</span></span>

<span data-ttu-id="d5296-210">Além de associação para valores, você também pode usar praticamente qualquer expressão JavaScript válido dentro de uma associação.</span><span class="sxs-lookup"><span data-stu-id="d5296-210">In addition to binding to values, you can also use almost any legal JavaScript expression within a binding.</span></span> <span data-ttu-id="d5296-211">Por exemplo, se um elemento de interface do usuário deve aparecer apenas em determinadas condições, como quando um valor excede um certo limite, você pode especificar isso logicamente dentro da expressão de associação:</span><span class="sxs-lookup"><span data-stu-id="d5296-211">For example, if a UI element should only appear under certain conditions, such as when a value exceeds a certain threshold, you can specify this logically within the binding expression:</span></span>

```html
<div data-bind="visible: customerValue > 100"></div>
```

<span data-ttu-id="d5296-212">Isso `<div>` só ficará visível quando o customerValue estiver acima de 100.</span><span class="sxs-lookup"><span data-stu-id="d5296-212">This `<div>` will only be visible when the customerValue is over 100.</span></span>

## <a name="templates"></a><span data-ttu-id="d5296-213">Modelos</span><span class="sxs-lookup"><span data-stu-id="d5296-213">Templates</span></span>

<span data-ttu-id="d5296-214">Knockout tem suporte para modelos, para que você pode separar facilmente sua interface do usuário de seu comportamento ou carregar incrementalmente os elementos de interface do usuário em um grande aplicativo sob demanda.</span><span class="sxs-lookup"><span data-stu-id="d5296-214">Knockout has support for templates, so that you can easily separate your UI from your behavior, or incrementally load UI elements into a large application on demand.</span></span> <span data-ttu-id="d5296-215">Podemos atualizar nosso exemplo anterior para tornar seu próprio modelo de cada linha simplesmente efetuando pull HTML out em um modelo e especificando o modelo por nome na chamada de associação de dados em `<tbody>`.</span><span class="sxs-lookup"><span data-stu-id="d5296-215">We can update our previous example to make each row its own template by simply pulling the HTML out into a template and specifying the template by name in the data-bind call on `<tbody>`.</span></span>

```html
<tbody data-bind="template: { name: 'rowTemplate', foreach: gameResults }">
</tbody>
<script type="text/html" id="rowTemplate">
  <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
  </tr>
</script>
```

<span data-ttu-id="d5296-216">Knockout também oferece suporte a outros mecanismos de aplicação de modelos, como a biblioteca de jQuery.tmpl e mecanismo de modelagem do Underscore.js.</span><span class="sxs-lookup"><span data-stu-id="d5296-216">Knockout also supports other templating engines, such as the jQuery.tmpl library and Underscore.js's templating engine.</span></span>

## <a name="components"></a><span data-ttu-id="d5296-217">Componentes</span><span class="sxs-lookup"><span data-stu-id="d5296-217">Components</span></span>

<span data-ttu-id="d5296-218">Componentes permitem organizar e reutilizar o código de interface do usuário, normalmente junto com os dados de ViewModel dos quais depende o código de interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="d5296-218">Components allow you to organize and reuse UI code, usually along with the ViewModel data on which the UI code depends.</span></span> <span data-ttu-id="d5296-219">Para criar um componente, basta especificar o seu modelo e seu viewModel e dê a ele um nome.</span><span class="sxs-lookup"><span data-stu-id="d5296-219">To create a component, you simply need to specify its template and its viewModel, and give it a name.</span></span> <span data-ttu-id="d5296-220">Isso é feito chamando `ko.components.register()`.</span><span class="sxs-lookup"><span data-stu-id="d5296-220">This is done by calling `ko.components.register()`.</span></span> <span data-ttu-id="d5296-221">Além de definir os modelos e viewmodel embutido, eles podem ser carregados de arquivos externos usando uma biblioteca como *require.js*, resultando em código muito limpo e eficiente.</span><span class="sxs-lookup"><span data-stu-id="d5296-221">In addition to defining the templates and viewmodel inline, they can be loaded from external files using a library like *require.js*, resulting in very clean and efficient code.</span></span>

## <a name="communicating-with-apis"></a><span data-ttu-id="d5296-222">Comunicação com APIs</span><span class="sxs-lookup"><span data-stu-id="d5296-222">Communicating with APIs</span></span>

<span data-ttu-id="d5296-223">Knockout pode trabalhar com os dados no formato JSON.</span><span class="sxs-lookup"><span data-stu-id="d5296-223">Knockout can work with any data in JSON format.</span></span> <span data-ttu-id="d5296-224">É uma maneira comum para recuperar e salvar dados usando o Knockout com jQuery, que oferece suporte a `$.getJSON()` função para recuperar dados e o `$.post()` método para enviar dados do navegador para um ponto de extremidade de API.</span><span class="sxs-lookup"><span data-stu-id="d5296-224">A common way to retrieve and save data using Knockout is with jQuery, which supports the `$.getJSON()` function to retrieve data, and the `$.post()` method to send data from the browser to an API endpoint.</span></span> <span data-ttu-id="d5296-225">É claro que, se você preferir um modo diferente para enviar e receber dados JSON, Knockout também funciona com ele.</span><span class="sxs-lookup"><span data-stu-id="d5296-225">Of course, if you prefer a different way to send and receive JSON data, Knockout will work with it as well.</span></span>

## <a name="summary"></a><span data-ttu-id="d5296-226">Resumo</span><span class="sxs-lookup"><span data-stu-id="d5296-226">Summary</span></span>

<span data-ttu-id="d5296-227">Knockout fornece uma maneira simples e discreta para associar os elementos de interface do usuário para o estado atual do aplicativo cliente, definido em um ViewModel.</span><span class="sxs-lookup"><span data-stu-id="d5296-227">Knockout provides a simple, elegant way to bind UI elements to the current state of the client application, defined in a ViewModel.</span></span> <span data-ttu-id="d5296-228">Sintaxe de associação do Knockout usa o atributo de associação de dados, aplicado a elementos HTML que serão processados.</span><span class="sxs-lookup"><span data-stu-id="d5296-228">Knockout's binding syntax uses the data-bind attribute, applied to HTML elements that are to be processed.</span></span> <span data-ttu-id="d5296-229">Separação é capaz de renderizar e atualizar grandes conjuntos de dados, rastreando a elementos de interface do usuário com eficiência e processando apenas alterações afetados elementos.</span><span class="sxs-lookup"><span data-stu-id="d5296-229">Knockout is able to efficiently render and update large data sets by tracking UI elements and only processing changes to affected elements.</span></span> <span data-ttu-id="d5296-230">Aplicativos grandes podem dividir a lógica de interface do usuário usando modelos e componentes, que podem ser carregados sob demanda de arquivos externos.</span><span class="sxs-lookup"><span data-stu-id="d5296-230">Large applications can break up UI logic using templates and components, which can be loaded on demand from external files.</span></span> <span data-ttu-id="d5296-231">Atualmente versão 3, Knockout é uma biblioteca JavaScript estável que pode melhorar a aplicativos da web que exigem a interatividade de cliente avançado.</span><span class="sxs-lookup"><span data-stu-id="d5296-231">Currently version 3, Knockout is a stable JavaScript library that can improve web applications that require rich client interactivity.</span></span>

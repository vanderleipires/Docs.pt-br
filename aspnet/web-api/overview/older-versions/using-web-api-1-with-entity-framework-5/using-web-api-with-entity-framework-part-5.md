---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Parte 5: Criando uma interface do usuário dinâmica com Knockout. js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: d019941700992e173a5812b11b558b6726dfd809
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834067"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="5ef81-102">Parte 5: Criando uma interface do usuário dinâmica com Knockout. js</span><span class="sxs-lookup"><span data-stu-id="5ef81-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>
====================
<span data-ttu-id="5ef81-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5ef81-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="5ef81-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="5ef81-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="5ef81-105">Criando uma interface do usuário dinâmica com Knockout. js</span><span class="sxs-lookup"><span data-stu-id="5ef81-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="5ef81-106">Nesta seção, vamos usar Knockout. js para adicionar funcionalidade para a exibição de administração.</span><span class="sxs-lookup"><span data-stu-id="5ef81-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="5ef81-107">[Knockout. js](http://knockoutjs.com/) é uma biblioteca de Javascript que torna mais fácil associar controles HTML aos dados.</span><span class="sxs-lookup"><span data-stu-id="5ef81-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="5ef81-108">Knockout. js usa o padrão Model-View-ViewModel (MVVM).</span><span class="sxs-lookup"><span data-stu-id="5ef81-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="5ef81-109">O *modelo* é a representação do lado do servidor dos dados no domínio de negócios (no nosso caso, produtos e pedidos).</span><span class="sxs-lookup"><span data-stu-id="5ef81-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="5ef81-110">O *exibição* é a camada de apresentação (HTML).</span><span class="sxs-lookup"><span data-stu-id="5ef81-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="5ef81-111">O *modelo de exibição* é um objeto Javascript que contém os dados de modelo.</span><span class="sxs-lookup"><span data-stu-id="5ef81-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="5ef81-112">O modelo de exibição é uma abstração de código da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="5ef81-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="5ef81-113">Ele não tem conhecimento da representação HTML.</span><span class="sxs-lookup"><span data-stu-id="5ef81-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="5ef81-114">Em vez disso, ele representa recursos abstratos da exibição, como "uma lista de itens".</span><span class="sxs-lookup"><span data-stu-id="5ef81-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="5ef81-115">O modo de exibição associado a dados para o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="5ef81-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="5ef81-116">Atualizações para o modelo de exibição são refletidas automaticamente no modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="5ef81-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="5ef81-117">O modelo de exibição também obtém os eventos de exibição, como cliques de botão e executa operações no modelo, como a criação de um pedido.</span><span class="sxs-lookup"><span data-stu-id="5ef81-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="5ef81-118">Primeiro, vamos definir o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="5ef81-118">First we'll define the view-model.</span></span> <span data-ttu-id="5ef81-119">Depois disso, podemos associará a marcação HTML para o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="5ef81-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="5ef81-120">Adicione a seguinte seção de Razor para Admin.cshtml:</span><span class="sxs-lookup"><span data-stu-id="5ef81-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="5ef81-121">Você pode adicionar esta seção em qualquer lugar no arquivo.</span><span class="sxs-lookup"><span data-stu-id="5ef81-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="5ef81-122">Quando o modo de exibição é processado, a seção aparece na parte inferior da página HTML, botão direito do mouse antes do fechamento &lt;/body&gt; marca.</span><span class="sxs-lookup"><span data-stu-id="5ef81-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="5ef81-123">Todos os scripts para esta página irão dentro da marca de script indicada pelo comentário:</span><span class="sxs-lookup"><span data-stu-id="5ef81-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="5ef81-124">Primeiro, defina uma classe de modelo de exibição:</span><span class="sxs-lookup"><span data-stu-id="5ef81-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="5ef81-125">**ko.observableArray** é um tipo especial de objeto no Knockout, chamado de um *observável*.</span><span class="sxs-lookup"><span data-stu-id="5ef81-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="5ef81-126">Dos [documentação do Knockout. js](http://knockoutjs.com/documentation/observables.html): um observável é um "objeto JavaScript que pode notificar assinantes sobre as alterações."</span><span class="sxs-lookup"><span data-stu-id="5ef81-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="5ef81-127">Quando altera o conteúdo de um observável, o modo de exibição é atualizado automaticamente para corresponder.</span><span class="sxs-lookup"><span data-stu-id="5ef81-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="5ef81-128">Para popular o `products` de matriz, faça uma solicitação AJAX a API da web.</span><span class="sxs-lookup"><span data-stu-id="5ef81-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="5ef81-129">Lembre-se de que armazenamos o URI de base para a API no recipiente de modo de exibição (consulte [parte 4](using-web-api-with-entity-framework-part-4.md) do tutorial).</span><span class="sxs-lookup"><span data-stu-id="5ef81-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="5ef81-130">Em seguida, adicione funções para o modelo de exibição para criar, atualizar e excluir produtos.</span><span class="sxs-lookup"><span data-stu-id="5ef81-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="5ef81-131">Essas funções enviar chamadas AJAX para a API da web e usam os resultados para atualizar o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="5ef81-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="5ef81-132">Agora a parte mais importante: quando DOM é fulled chamada carregada, o **ko.applyBindings** de função e passar uma nova instância do `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="5ef81-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="5ef81-133">O **ko.applyBindings** método ativa o Knockout e conecta o modelo de exibição para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="5ef81-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="5ef81-134">Agora que temos um modelo de exibição, podemos criar as associações.</span><span class="sxs-lookup"><span data-stu-id="5ef81-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="5ef81-135">No Knockout. js, faça isso adicionando `data-bind` atributos para elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="5ef81-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="5ef81-136">Por exemplo, para associar uma lista de HTML para uma matriz, use o `foreach` associação:</span><span class="sxs-lookup"><span data-stu-id="5ef81-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="5ef81-137">O `foreach` associação itera na matriz e cria filho elementos para cada objeto na matriz.</span><span class="sxs-lookup"><span data-stu-id="5ef81-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="5ef81-138">Associações em elementos filho podem se referir às propriedades nos objetos de matriz.</span><span class="sxs-lookup"><span data-stu-id="5ef81-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="5ef81-139">Adicione as seguintes associações à lista de "produtos de atualização":</span><span class="sxs-lookup"><span data-stu-id="5ef81-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="5ef81-140">O `<li>` elemento ocorre dentro do escopo do **foreach** associação.</span><span class="sxs-lookup"><span data-stu-id="5ef81-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="5ef81-141">Que significa que será o Knockout renderizar o elemento de uma vez para cada produto no `products` matriz.</span><span class="sxs-lookup"><span data-stu-id="5ef81-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="5ef81-142">Todas as associações dentro a `<li>` elemento se referir a essa instância do produto.</span><span class="sxs-lookup"><span data-stu-id="5ef81-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="5ef81-143">Por exemplo, `$data.Name` refere-se ao `Name` propriedade no produto.</span><span class="sxs-lookup"><span data-stu-id="5ef81-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="5ef81-144">Para definir os valores das entradas de texto, use o `value` associação.</span><span class="sxs-lookup"><span data-stu-id="5ef81-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="5ef81-145">Os botões são associados a funções na exibição do modelo, usando o `click` associação.</span><span class="sxs-lookup"><span data-stu-id="5ef81-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="5ef81-146">A instância de produto é passada como um parâmetro para cada função.</span><span class="sxs-lookup"><span data-stu-id="5ef81-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="5ef81-147">Para obter mais informações, o [documentação do Knockout. js](http://knockoutjs.com/documentation/observables.html) tem boa descrições das várias associações.</span><span class="sxs-lookup"><span data-stu-id="5ef81-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="5ef81-148">Em seguida, adicione uma associação para o **enviar** eventos no formulário Adicionar produto:</span><span class="sxs-lookup"><span data-stu-id="5ef81-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="5ef81-149">Essa associação chama o `create` função no modelo de exibição para criar um novo produto.</span><span class="sxs-lookup"><span data-stu-id="5ef81-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="5ef81-150">Aqui está o código completo para o modo de exibição do administrador:</span><span class="sxs-lookup"><span data-stu-id="5ef81-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="5ef81-151">Executar o aplicativo, faça logon com a conta de administrador e clique no link "Admin".</span><span class="sxs-lookup"><span data-stu-id="5ef81-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="5ef81-152">Você deve ver a lista de produtos e ser capaz de criar, atualizar ou excluir produtos.</span><span class="sxs-lookup"><span data-stu-id="5ef81-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5ef81-153">[Anterior](using-web-api-with-entity-framework-part-4.md)
> [Próximo](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="5ef81-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>

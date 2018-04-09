---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Parte 7: Criar principal página | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 2c378e68a1e6600daf655c19afbfe355e89496d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="part-7-creating-the-main-page"></a><span data-ttu-id="48de3-102">Parte 7: Criar principal página</span><span class="sxs-lookup"><span data-stu-id="48de3-102">Part 7: Creating the Main Page</span></span>
====================
<span data-ttu-id="48de3-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="48de3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="48de3-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="48de3-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="48de3-105">Criando principal página</span><span class="sxs-lookup"><span data-stu-id="48de3-105">Creating the Main Page</span></span>

<span data-ttu-id="48de3-106">Nesta seção, você criará a página principal do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="48de3-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="48de3-107">Esta página será mais complexa do que a página de administração, portanto será a abordamos em várias etapas.</span><span class="sxs-lookup"><span data-stu-id="48de3-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="48de3-108">Ao longo do processo, você verá algumas técnicas mais avançadas do Knockout. js.</span><span class="sxs-lookup"><span data-stu-id="48de3-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="48de3-109">Aqui está o layout básico da página:</span><span class="sxs-lookup"><span data-stu-id="48de3-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="48de3-110">"Produtos" contém uma matriz de produtos.</span><span class="sxs-lookup"><span data-stu-id="48de3-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="48de3-111">"Carrinho" contém uma matriz de produtos com quantidades.</span><span class="sxs-lookup"><span data-stu-id="48de3-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="48de3-112">Clicando em "Adicionar ao carrinho" atualiza o carrinho.</span><span class="sxs-lookup"><span data-stu-id="48de3-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="48de3-113">"Orders" contém uma matriz de IDs do pedido.</span><span class="sxs-lookup"><span data-stu-id="48de3-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="48de3-114">"Detalhes" contém um detalhe de ordem, que é uma matriz de itens (produtos com quantidades)</span><span class="sxs-lookup"><span data-stu-id="48de3-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="48de3-115">Vamos começar definindo algumas layout básico em HTML, sem associação de dados ou script.</span><span class="sxs-lookup"><span data-stu-id="48de3-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="48de3-116">Abra o arquivo Views/Home/Index.cshtml e substitua todo o conteúdo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="48de3-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="48de3-117">Em seguida, adicionar uma seção de Scripts e criar um modelo vazio de exibição:</span><span class="sxs-lookup"><span data-stu-id="48de3-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="48de3-118">Com base no design elaborado anteriormente, nosso modelo de exibição precisa observáveis carrinho, produtos, pedidos e detalhes.</span><span class="sxs-lookup"><span data-stu-id="48de3-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="48de3-119">Adicione as seguintes variáveis para o `AppViewModel` objeto:</span><span class="sxs-lookup"><span data-stu-id="48de3-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="48de3-120">Os usuários podem adicionar itens de lista de produtos no carrinho de e remover itens do carrinho.</span><span class="sxs-lookup"><span data-stu-id="48de3-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="48de3-121">Para encapsular essas funções, vamos criar outra classe de modelo de exibição que representa um produto.</span><span class="sxs-lookup"><span data-stu-id="48de3-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="48de3-122">Adicione o seguinte código ao `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="48de3-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="48de3-123">O `ProductViewModel` classe contém duas funções que são usadas para mover o produto para e do carrinho: `addItemToCart` adiciona uma unidade do produto ao carrinho, e `removeAllFromCart` remove todas as quantidades de produto.</span><span class="sxs-lookup"><span data-stu-id="48de3-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="48de3-124">Os usuários podem selecionar um pedido existente e obter os detalhes do pedido.</span><span class="sxs-lookup"><span data-stu-id="48de3-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="48de3-125">Podemos será encapsular essa funcionalidade em outro modelo de exibição:</span><span class="sxs-lookup"><span data-stu-id="48de3-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="48de3-126">O `OrderDetailsViewModel` é inicializada com uma ordem, e ele busca dos detalhes do pedido, enviando uma solicitação AJAX para o servidor.</span><span class="sxs-lookup"><span data-stu-id="48de3-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="48de3-127">Além disso, observe o `total` propriedade o `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="48de3-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="48de3-128">Esta propriedade é um tipo especial de observável chamado um [computada observável](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="48de3-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="48de3-129">Como o nome implica, um computada observável permite associação de dados para um valor computado&#8212;nesse caso, o total de custo da ordem.</span><span class="sxs-lookup"><span data-stu-id="48de3-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="48de3-130">Em seguida, adicionar essas funções para `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="48de3-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="48de3-131">`resetCart` Remove todos os itens do carrinho.</span><span class="sxs-lookup"><span data-stu-id="48de3-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="48de3-132">`getDetails` Obtém os detalhes de um pedido (por pusing um novo `OrderDetailsViewModel` até o `details` lista).</span><span class="sxs-lookup"><span data-stu-id="48de3-132">`getDetails` gets the details for an order (by pusing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="48de3-133">`createOrder` cria um novo pedido e esvazia o carrinho.</span><span class="sxs-lookup"><span data-stu-id="48de3-133">`createOrder` creates a new order and empties the cart.</span></span>


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="48de3-134">Finalmente, inicialize o modelo de exibição, fazendo solicitações do AJAX para os produtos e pedidos:</span><span class="sxs-lookup"><span data-stu-id="48de3-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="48de3-135">Okey, que é muito código, mas podemos compilá-lo passo a passo, esperamos o design é criptografado.</span><span class="sxs-lookup"><span data-stu-id="48de3-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="48de3-136">Agora podemos adicionar algumas associações Knockout. js para HTML.</span><span class="sxs-lookup"><span data-stu-id="48de3-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="48de3-137">**Produtos**</span><span class="sxs-lookup"><span data-stu-id="48de3-137">**Products**</span></span>

<span data-ttu-id="48de3-138">Aqui estão as ligações para a lista de produtos:</span><span class="sxs-lookup"><span data-stu-id="48de3-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="48de3-139">Isso é iterado sobre a matriz de produtos e exibe o nome e o preço.</span><span class="sxs-lookup"><span data-stu-id="48de3-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="48de3-140">O botão "Adicionar a ordem" só é visível quando o usuário está conectado.</span><span class="sxs-lookup"><span data-stu-id="48de3-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="48de3-141">As chamadas do botão "Adicionar a ordem" `addItemToCart` no `ProductViewModel` instância para o produto.</span><span class="sxs-lookup"><span data-stu-id="48de3-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="48de3-142">Isso demonstra um recurso interessante de Knockout. js: quando um modelo de exibição contiver outros modelos de exibição, você pode aplicar as ligações para o modelo interno.</span><span class="sxs-lookup"><span data-stu-id="48de3-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="48de3-143">Neste exemplo, as associações de dentro do `foreach` são aplicadas a cada uma da `ProductViewModel` instâncias.</span><span class="sxs-lookup"><span data-stu-id="48de3-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="48de3-144">Essa abordagem é muito mais fácil do que colocar todos os recursos em um único modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="48de3-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="48de3-145">**Cart**</span><span class="sxs-lookup"><span data-stu-id="48de3-145">**Cart**</span></span>

<span data-ttu-id="48de3-146">Aqui estão as associações de carrinho de:</span><span class="sxs-lookup"><span data-stu-id="48de3-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="48de3-147">Isso é iterado sobre a matriz de carrinho e exibe o nome, preço e quantidade.</span><span class="sxs-lookup"><span data-stu-id="48de3-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="48de3-148">Observe que o link "Remover" e o botão "Criar pedido" estão associados a funções de modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="48de3-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="48de3-149">**Pedidos**</span><span class="sxs-lookup"><span data-stu-id="48de3-149">**Orders**</span></span>

<span data-ttu-id="48de3-150">Aqui estão as ligações para obter a lista de pedidos:</span><span class="sxs-lookup"><span data-stu-id="48de3-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="48de3-151">Isso é iterado sobre os pedidos e mostra a ID da ordem.</span><span class="sxs-lookup"><span data-stu-id="48de3-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="48de3-152">O evento de clique no link está vinculado para o `getDetails` função.</span><span class="sxs-lookup"><span data-stu-id="48de3-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="48de3-153">**Detalhes do pedido**</span><span class="sxs-lookup"><span data-stu-id="48de3-153">**Order Details**</span></span>

<span data-ttu-id="48de3-154">Aqui estão as associações com os detalhes do pedido:</span><span class="sxs-lookup"><span data-stu-id="48de3-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="48de3-155">Isso é iterado sobre os itens na ordem e exibe o produto, preço e quantidade.</span><span class="sxs-lookup"><span data-stu-id="48de3-155">This iterates over the items in the order and displays the product, price, and quanity.</span></span> <span data-ttu-id="48de3-156">Div ao redor é visível somente se a matriz de detalhes contém um ou mais itens.</span><span class="sxs-lookup"><span data-stu-id="48de3-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="48de3-157">Conclusão</span><span class="sxs-lookup"><span data-stu-id="48de3-157">Conclusion</span></span>

<span data-ttu-id="48de3-158">Neste tutorial, você criou um aplicativo que usa o Entity Framework para se comunicar com o banco de dados e a API Web do ASP.NET para fornecer uma interface de público sobre a camada de dados.</span><span class="sxs-lookup"><span data-stu-id="48de3-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="48de3-159">Podemos usar o ASP.NET MVC 4 para renderizar páginas HTML e Knockout. js e jQuery para oferecer interações dinâmicas sem recargas de página.</span><span class="sxs-lookup"><span data-stu-id="48de3-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="48de3-160">Recursos adicionais:</span><span class="sxs-lookup"><span data-stu-id="48de3-160">Additional resources:</span></span>

- [<span data-ttu-id="48de3-161">Mapa de conteúdo de acesso de dados do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="48de3-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="48de3-162">Entity Framework Developer Center</span><span class="sxs-lookup"><span data-stu-id="48de3-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="48de3-163">Anterior</span><span class="sxs-lookup"><span data-stu-id="48de3-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)

---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Parte 8: Carrinho com atualizações do Ajax de compras | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 8 aborda o carrinho de compras com atualizações do Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 327b7ee4e302188323c229c231ae750cbed709a1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369790"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="3ae4c-104">Parte 8: Carrinho de compras com atualizações do Ajax</span><span class="sxs-lookup"><span data-stu-id="3ae4c-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="3ae4c-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="3ae4c-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="3ae4c-106">A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="3ae4c-107">A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="3ae4c-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="3ae4c-109">Parte 8 aborda o carrinho de compras com atualizações do Ajax.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="3ae4c-110">Permitiremos que os usuários a colocar álbuns no carrinho sem registro, mas precisará se registrar como convidados fazer check-out completa.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="3ae4c-111">O processo de compras e check-out será separado em dois controladores: um controlador de ShoppingCart que permite anonimamente adicionar itens a um carrinho e um controlador de check-out que manipula o processo de check-out.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="3ae4c-112">Podemos vai começar com o carrinho de compras nesta seção, em seguida, crie o processo de check-out na seção a seguir.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="3ae4c-113">Adicionando as classes de modelo carrinho, Order e OrderDetail</span><span class="sxs-lookup"><span data-stu-id="3ae4c-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="3ae4c-114">Nossos processos de carrinho de compras e check-out serão fazer uso de algumas novas classes.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="3ae4c-115">Clique com botão direito na pasta modelos e adicionar uma classe de carrinho (Cart.cs) com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="3ae4c-116">Essa classe é muito semelhante a outras pessoas que usamos até agora, com o atributo [chave] para a propriedade RecordId a exceção.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="3ae4c-117">Os itens do carrinho terá um identificador de cadeia de caracteres chamado CartID para permitir que compras anônimas, mas a tabela inclui uma chave primária de inteiro chamada RecordId.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="3ae4c-118">Por convenção, o Entity Framework Code-First espera que a chave primária para uma tabela chamada carrinho será CartId ou ID, mas podemos pode facilmente substituir isso por meio de anotações ou código se quisermos.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="3ae4c-119">Este é um exemplo de como podemos usar as convenções simples no Entity Framework Code-First quando eles se adequar nos, mas nós não estão restringidos pelas-los quando não necessitarem.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="3ae4c-120">Em seguida, adicione uma classe de ordem ({1&gt;Order.CS&lt;1) com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="3ae4c-121">Esta classe controla informações de resumo e a entrega de um pedido.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="3ae4c-122">**Ele não será compilado ainda**, pois ele tem uma propriedade de navegação OrderDetails que depende de uma classe que ainda não criamos ainda.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="3ae4c-123">Vamos corrigir que agora com a adição de uma classe denominada OrderDetail.cs, adicionando o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="3ae4c-124">Faremos uma última atualização à nossa classe MusicStoreEntities incluir DbSets que expõem essas novas classes de modelo, incluindo também um DbSet&lt;artista&gt;.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="3ae4c-125">A classe MusicStoreEntities atualizada é exibido como abaixo.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="3ae4c-126">Gerenciando a lógica de negócios do carrinho de compras</span><span class="sxs-lookup"><span data-stu-id="3ae4c-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="3ae4c-127">Em seguida, vamos criar a classe ShoppingCart na pasta de modelos.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="3ae4c-128">O modelo de ShoppingCart lida com acesso a dados na tabela do carrinho.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="3ae4c-129">Além disso, ele irá manipular a lógica de negócios para adicionar e remover itens do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="3ae4c-130">Uma vez que não queremos exigir que os usuários para se inscrever para uma conta apenas adicionar itens ao carrinho de compras, atribuiremos usuários um identificador exclusivo temporário (usando um GUID ou o identificador global exclusivo) quando acessarem o carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="3ae4c-131">Armazenaremos essa ID usando a classe de sessão do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="3ae4c-132">*Observação: A sessão do ASP.NET é um local conveniente para armazenar informações específicas do usuário que irá expirar depois que sair do site. Embora o uso indevido do estado da sessão pode ter implicações de desempenho em sites maiores, nosso uso de luz funcionará bem para fins de demonstração.*</span><span class="sxs-lookup"><span data-stu-id="3ae4c-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="3ae4c-133">A classe ShoppingCart expõe os métodos a seguir:</span><span class="sxs-lookup"><span data-stu-id="3ae4c-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="3ae4c-134">**AddToCart** leva um álbum como um parâmetro e o adiciona ao carrinho do usuário.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="3ae4c-135">Uma vez que a tabela de carrinho controla a quantidade para cada álbum, inclui a lógica para criar uma nova linha, se necessário, ou apenas incrementar a quantidade, se o usuário já tiver solicitada para uma cópia do álbum.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="3ae4c-136">**RemoveFromCart** usa uma ID de álbum e a remove do carrinho do usuário.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="3ae4c-137">Se o usuário tiver apenas uma cópia do álbum em seu carrinho, a linha é removida.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="3ae4c-138">**EmptyCart** remove todos os itens do carrinho de compras do usuário.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="3ae4c-139">**GetCartItems** recupera uma lista de CartItems para exibição ou processamento.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="3ae4c-140">**GetCount** recupera um número total de álbuns de um usuário tem em seu carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="3ae4c-141">**GetTotal genérica** calcula o custo total de todos os itens no carrinho.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="3ae4c-142">**CreateOrder** converte o carrinho de compras em uma ordem durante a fase de check-out.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="3ae4c-143">**GetCart** é um método estático que permite que os controladores obter um objeto de carrinho.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="3ae4c-144">Ele usa o **GetCartId** método para manipular o CartId lendo da sessão do usuário.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="3ae4c-145">O método GetCartId requer o HttpContextBase para que ele possa ler CartId do usuário da sessão do usuário.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="3ae4c-146">Aqui está o completo **ShoppingCart classe**:</span><span class="sxs-lookup"><span data-stu-id="3ae4c-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="3ae4c-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="3ae4c-147">ViewModels</span></span>

<span data-ttu-id="3ae4c-148">Nosso controlador de carrinho de compras será preciso comunicar-se algumas informações complexas para seus modos de exibição que não é mapeado corretamente para nossos objetos de modelo.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="3ae4c-149">Não queremos modificar nossos modelos para atender às nossas opiniões; Classes de modelo devem representar o nosso domínio, não a interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="3ae4c-150">Uma solução seria passar as informações para nossos modos de exibição usando a classe ViewBag, como fizemos com as informações de lista suspensa do Store Manager, mas transmitir muitas informações por meio de ViewBag obtém difícil de gerenciar.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="3ae4c-151">Uma solução para isso é usar o *ViewModel* padrão.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="3ae4c-152">Ao usar esse padrão, podemos criar classes fortemente tipadas que são otimizados para nossos cenários de modo de exibição específico, e que expõem propriedades para o valores/conteúdo dinâmico necessários para os nossos modelos de exibição.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="3ae4c-153">Nossas classes de controlador podem, em seguida, preencher e passar essas classes de exibição otimizada para nosso modelo de exibição para usar.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="3ae4c-154">Isso permite que o tipo de segurança, a verificação de tempo de compilação e IntelliSense do editor dentro de modelos de exibição.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="3ae4c-155">Vamos criar dois modelos de exibição para uso no nosso controlador de carrinho de compras: o ShoppingCartViewModel conterá o conteúdo do carrinho de compras do usuário, e o ShoppingCartRemoveViewModel será usado para exibir informações de confirmação quando um usuário remove algo do seu carrinho.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="3ae4c-156">Vamos criar uma nova pasta ViewModels na raiz do nosso projeto para manter as coisas organizadas.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="3ae4c-157">Clique com botão direito no projeto, selecione Adicionar / nova pasta.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="3ae4c-158">Nomeie a pasta ViewModels.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="3ae4c-159">Em seguida, adicione a classe ShoppingCartViewModel na pasta ViewModels.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="3ae4c-160">Ele tem duas propriedades: uma lista de itens do carrinho e um valor decimal para manter o preço total para todos os itens no carrinho.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="3ae4c-161">Agora, adicione o ShoppingCartRemoveViewModel à pasta ViewModels, com as quatro propriedades a seguir.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="3ae4c-162">O controlador de carrinho de compras</span><span class="sxs-lookup"><span data-stu-id="3ae4c-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="3ae4c-163">O controlador de carrinho de compras tem três objetivos principais: adicionar itens a um carrinho, removendo itens do carrinho e exibir itens no carrinho de.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="3ae4c-164">Ele fará o uso das três classes, podemos acabou de criar: ShoppingCartViewModel, ShoppingCartRemoveViewModel e ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="3ae4c-165">Como no StoreController e StoreManagerController, vamos adicionar um campo para armazenar uma instância de MusicStoreEntities.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="3ae4c-166">Adicione um novo controlador de carrinho de compras para o projeto usando o modelo de controlador vazia.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="3ae4c-167">Aqui está o controlador ShoppingCart completa.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="3ae4c-168">As ações de índice e adicionar controlador devem ser bastante familiares.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="3ae4c-169">As ações do controlador remover e CartSummary manipular dois casos especiais, que discutiremos na seção a seguir.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="3ae4c-170">Atualizações do AJAX com o jQuery</span><span class="sxs-lookup"><span data-stu-id="3ae4c-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="3ae4c-171">Em seguida, criaremos uma página de índice de carrinho de compras que é fortemente tipada para o ShoppingCartViewModel e usa o modelo de exibição de lista usando o mesmo método que antes.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="3ae4c-172">No entanto, em vez de usar um HTML. ActionLink para remover itens do carrinho, vamos usar jQuery "conectar" para todos os links neste modo de exibição que têm a classe HTML RemoveLink o evento de clique.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="3ae4c-173">Em vez de lançar o formulário, esse manipulador de eventos click apenas fará um retorno de chamada do AJAX para nossa ação de controlador RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="3ae4c-174">O RemoveFromCart retorna um resultado serializado JSON, que nosso retorno de chamada do jQuery, em seguida, analisa e executa quatro atualizações rápidas para a página usando o jQuery:</span><span class="sxs-lookup"><span data-stu-id="3ae4c-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="3ae4c-175">Remove o álbum excluído da lista</span><span class="sxs-lookup"><span data-stu-id="3ae4c-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="3ae4c-176">Atualiza a contagem de carrinho no cabeçalho</span><span class="sxs-lookup"><span data-stu-id="3ae4c-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="3ae4c-177">Exibe uma mensagem de atualização para o usuário</span><span class="sxs-lookup"><span data-stu-id="3ae4c-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="3ae4c-178">Atualiza o preço total do carrinho</span><span class="sxs-lookup"><span data-stu-id="3ae4c-178">Updates the cart total price</span></span>

<span data-ttu-id="3ae4c-179">Uma vez que o cenário de remoção é manipulado por um retorno de chamada Ajax dentro da exibição de índice, não precisamos um modo de exibição adicional para a ação de RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="3ae4c-180">Aqui está o código completo para o modo de exibição /ShoppingCart/Index:</span><span class="sxs-lookup"><span data-stu-id="3ae4c-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="3ae4c-181">Para testar isso, precisamos ser capazes de adicionar itens ao nosso carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="3ae4c-182">Vamos atualizar nossos **Store detalhes** modo de exibição para incluir um botão "Adicionar ao carrinho".</span><span class="sxs-lookup"><span data-stu-id="3ae4c-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="3ae4c-183">Enquanto estamos fazendo isso, podemos incluir algumas informações adicionais álbum que adicionamos, pois foi atualizado pela última vez nesta exibição: arte do álbum, artista, preço e gênero.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="3ae4c-184">O código de modo de exibição de detalhes de Store atualizado é exibido conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="3ae4c-185">Agora podemos clique por meio da loja e adicionando e removendo álbuns para e do nosso carrinho de compras de teste.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="3ae4c-186">Execute o aplicativo e navegue até o índice de Store.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="3ae4c-187">Em seguida, clique em um gênero para exibir uma lista dos álbuns.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="3ae4c-188">Clicar em um título de álbum agora mostra nossa exibição de detalhes do álbum atualizada, incluindo o botão "Adicionar ao carrinho".</span><span class="sxs-lookup"><span data-stu-id="3ae4c-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="3ae4c-189">Clicar no botão "Adicionar ao carrinho" mostra nossa exibição índice de carrinho de compras com a lista de resumo de carrinho compras.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="3ae4c-190">Após o carregamento de carrinho de compras, você pode clicar na remover do link de carrinho para ver a atualização do Ajax ao carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="3ae4c-191">Criamos um carrinho que permite que os usuários não registrados adicionar itens ao carrinho de compras em funcionamento.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="3ae4c-192">Na seção a seguir, permitiremos que eles possam registrar e concluir o processo de check-out.</span><span class="sxs-lookup"><span data-stu-id="3ae4c-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="3ae4c-193">[Anterior](mvc-music-store-part-7.md)
> [Próximo](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="3ae4c-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>

---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Parte 8: Shopping Cart com atualizações Ajax | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 8 abrange o carrinho de compras com atualizações Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 195c01ff0d71b2bfd0c00e71244d47a166330921
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871279"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="32785-104">Parte 8: Carrinho de compras com atualizações Ajax</span><span class="sxs-lookup"><span data-stu-id="32785-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="32785-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="32785-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="32785-106">O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.</span><span class="sxs-lookup"><span data-stu-id="32785-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="32785-107">O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="32785-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="32785-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="32785-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="32785-109">Parte 8 abrange o carrinho de compras com atualizações Ajax.</span><span class="sxs-lookup"><span data-stu-id="32785-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="32785-110">Vai permitir que os usuários a colocar álbuns no carrinho sem registrar, mas será necessário registrar como convidados completa check-out.</span><span class="sxs-lookup"><span data-stu-id="32785-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="32785-111">O processo de compras e check-out será separado em dois controladores: um controlador ShoppingCart que permite anonimamente adicionar itens a um carrinho e um controlador de check-out, que gerencia o processo de check-out.</span><span class="sxs-lookup"><span data-stu-id="32785-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="32785-112">Podemos vai começar com o carrinho de compras nesta seção, em seguida, crie o processo de check-out na seção a seguir.</span><span class="sxs-lookup"><span data-stu-id="32785-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="32785-113">Adicionando classes de modelo do carrinho, pedido e OrderDetail</span><span class="sxs-lookup"><span data-stu-id="32785-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="32785-114">Nossos processos de carrinho de compras e check-out fará com que o uso de algumas novas classes.</span><span class="sxs-lookup"><span data-stu-id="32785-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="32785-115">Com o botão direito na pasta modelos e adicione uma classe de carrinho (Cart.cs) com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="32785-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="32785-116">Essa classe é muito semelhante a outras pessoas que usamos até agora, com a exceção do atributo [chave] para a propriedade RecordId.</span><span class="sxs-lookup"><span data-stu-id="32785-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="32785-117">Os itens do carrinho terá um identificador de cadeia de caracteres chamado CartID para permitir compras anônima, mas a tabela inclui uma chave primária de inteiro nomeada RecordId.</span><span class="sxs-lookup"><span data-stu-id="32785-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="32785-118">Por convenção, o Entity Framework Code-First espera que a chave primária para uma tabela chamada carrinho será CartId ou ID, mas podemos facilmente substituir que via anotações ou código se quisermos.</span><span class="sxs-lookup"><span data-stu-id="32785-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="32785-119">Este é um exemplo de como podemos usar as convenções simples no Entity Framework Code-First quando eles se adequar nos, mas não está restrito por elas quando eles não.</span><span class="sxs-lookup"><span data-stu-id="32785-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="32785-120">Em seguida, adicione uma classe de ordem (Order.cs) com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="32785-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="32785-121">Esta classe controla informações de resumo e entrega de um pedido.</span><span class="sxs-lookup"><span data-stu-id="32785-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="32785-122">**Ele não será compilado ainda**, pois ele tem uma propriedade de navegação OrderDetails que depende de uma classe que ainda não tenha criado.</span><span class="sxs-lookup"><span data-stu-id="32785-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="32785-123">Vamos corrigir que agora com a adição de uma classe denominada OrderDetail.cs, adicionando o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="32785-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="32785-124">Será feita uma última atualização à nossa classe MusicStoreEntities incluir DbSets que expõem as novas classes de modelo, incluindo também um DbSet&lt;artista&gt;.</span><span class="sxs-lookup"><span data-stu-id="32785-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="32785-125">A classe MusicStoreEntities atualizada aparece como abaixo.</span><span class="sxs-lookup"><span data-stu-id="32785-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="32785-126">Gerenciando a lógica de negócios do carrinho de compras</span><span class="sxs-lookup"><span data-stu-id="32785-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="32785-127">Em seguida, vamos criar a classe ShoppingCart na pasta de modelos.</span><span class="sxs-lookup"><span data-stu-id="32785-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="32785-128">O modelo ShoppingCart lida com acesso a dados para a tabela de carrinho.</span><span class="sxs-lookup"><span data-stu-id="32785-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="32785-129">Além disso, ele tratará a lógica de negócios para adicionar e remover itens do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="32785-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="32785-130">Como não queremos que exija que os usuários se inscrever para uma conta apenas adicionar itens ao carrinho de compras, será atribuir usuários um identificador exclusivo temporário (usando um GUID ou o identificador global exclusivo) quando acessam o carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="32785-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="32785-131">Armazenamos essa ID usando a classe de sessão do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="32785-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="32785-132">*Observação: A sessão do ASP.NET é um local conveniente para armazenar informações específicas do usuário que irá expirar depois que sair do site. Enquanto o uso indevido do estado da sessão pode ter implicações de desempenho em sites maiores, usamos leve funciona bem para fins de demonstração.*</span><span class="sxs-lookup"><span data-stu-id="32785-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="32785-133">A classe ShoppingCart expõe os métodos a seguir:</span><span class="sxs-lookup"><span data-stu-id="32785-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="32785-134">**AddToCart** leva um álbum como um parâmetro e o adiciona ao carrinho do usuário.</span><span class="sxs-lookup"><span data-stu-id="32785-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="32785-135">Desde que a tabela de carrinho controla a quantidade de cada álbum, inclui a lógica para criar uma nova linha, se necessário, ou apenas incrementar a quantidade, se o usuário já tiver solicitado uma cópia do álbum.</span><span class="sxs-lookup"><span data-stu-id="32785-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="32785-136">**RemoveFromCart** usa uma ID de álbum e a remove do carrinho do usuário.</span><span class="sxs-lookup"><span data-stu-id="32785-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="32785-137">Se o usuário tinha apenas uma cópia do álbum em seu carrinho, a linha será removida.</span><span class="sxs-lookup"><span data-stu-id="32785-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="32785-138">**EmptyCart** remove todos os itens do carrinho de compras do usuário.</span><span class="sxs-lookup"><span data-stu-id="32785-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="32785-139">**GetCartItems** recupera uma lista de CartItems para processamento ou exibição.</span><span class="sxs-lookup"><span data-stu-id="32785-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="32785-140">**GetCount** recupera um número total de álbuns de um usuário tem em seu carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="32785-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="32785-141">**GetTotal genérica** calcula o custo total de todos os itens no carrinho.</span><span class="sxs-lookup"><span data-stu-id="32785-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="32785-142">**CreateOrder** converte o carrinho de compras em uma ordem durante a fase de check-out.</span><span class="sxs-lookup"><span data-stu-id="32785-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="32785-143">**GetCart** é um método estático que permite que os controladores obter um objeto de carrinho.</span><span class="sxs-lookup"><span data-stu-id="32785-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="32785-144">Ele usa o **GetCartId** método para tratar o CartId de leitura da sessão do usuário.</span><span class="sxs-lookup"><span data-stu-id="32785-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="32785-145">O método GetCartId requer o HttpContextBase para que ele possa ler CartId do usuário da sessão do usuário.</span><span class="sxs-lookup"><span data-stu-id="32785-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="32785-146">Aqui está o completo **ShoppingCart classe**:</span><span class="sxs-lookup"><span data-stu-id="32785-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="32785-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="32785-147">ViewModels</span></span>

<span data-ttu-id="32785-148">Nosso controlador de carrinho de compras precisa se comunicar algumas informações complexas para seus modos de exibição que não mapeiam corretamente para os objetos de modelo.</span><span class="sxs-lookup"><span data-stu-id="32785-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="32785-149">Não queremos modificar os modelos de acordo com os modos de exibição; Classes de modelo devem representar o domínio, não a interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="32785-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="32785-150">Uma solução seria passar as informações para os modos de exibição usando a classe ViewBag, fizemos com as informações de lista suspensa do Gerenciador de armazenamento, mas passar muitas informações via ViewBag obtém difíceis de gerenciar.</span><span class="sxs-lookup"><span data-stu-id="32785-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="32785-151">Uma solução para isso é usar o *ViewModel* padrão.</span><span class="sxs-lookup"><span data-stu-id="32785-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="32785-152">Ao usar esse padrão, podemos criar classes fortemente tipada que são otimizados para nossos cenários de modo de exibição específico e que expõem propriedades para valores/conteúdo dinâmico necessária para os nossos modelos de exibição.</span><span class="sxs-lookup"><span data-stu-id="32785-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="32785-153">Classes nosso controlador podem popular e passar essas classes otimizado para exibição para nosso modelo de exibição a ser usado.</span><span class="sxs-lookup"><span data-stu-id="32785-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="32785-154">Isso permite que a segurança de tipo, a verificação de tempo de compilação e IntelliSense do editor nos modelos de exibição.</span><span class="sxs-lookup"><span data-stu-id="32785-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="32785-155">Vamos criar dois modelos de exibição para uso em nosso controlador do carrinho de compras: o ShoppingCartViewModel conterá o conteúdo do carrinho de compras do usuário e o ShoppingCartRemoveViewModel será usado para exibir informações de confirmação quando um usuário remove algo de seu carrinho.</span><span class="sxs-lookup"><span data-stu-id="32785-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="32785-156">Vamos criar uma nova pasta ViewModels na raiz do projeto para manter a organização.</span><span class="sxs-lookup"><span data-stu-id="32785-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="32785-157">Clique com o botão direito, selecione Adicionar / nova pasta.</span><span class="sxs-lookup"><span data-stu-id="32785-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="32785-158">O nome da pasta ViewModels.</span><span class="sxs-lookup"><span data-stu-id="32785-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="32785-159">Em seguida, adicione a classe ShoppingCartViewModel na pasta ViewModels.</span><span class="sxs-lookup"><span data-stu-id="32785-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="32785-160">Ele tem duas propriedades: uma lista de itens do carrinho e um valor decimal para manter o preço total para todos os itens no carrinho.</span><span class="sxs-lookup"><span data-stu-id="32785-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="32785-161">Agora adicione o ShoppingCartRemoveViewModel para a pasta ViewModels, com as seguintes propriedades de quatro.</span><span class="sxs-lookup"><span data-stu-id="32785-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="32785-162">O controlador de carrinho de compras</span><span class="sxs-lookup"><span data-stu-id="32785-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="32785-163">O controlador de carrinho de compras tem três objetivos principais: adicionar itens a um carrinho, removendo itens do carrinho e exibir itens no carrinho de.</span><span class="sxs-lookup"><span data-stu-id="32785-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="32785-164">Ela fará o uso das três classes acabou de criar: ShoppingCartViewModel, ShoppingCartRemoveViewModel e ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="32785-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="32785-165">Como no StoreController e StoreManagerController, vamos adicionar um campo para manter uma instância do MusicStoreEntities.</span><span class="sxs-lookup"><span data-stu-id="32785-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="32785-166">Adicione um novo controlador de carrinho de compras para o projeto usando o modelo de controlador vazio.</span><span class="sxs-lookup"><span data-stu-id="32785-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="32785-167">Aqui está o controlador ShoppingCart concluída.</span><span class="sxs-lookup"><span data-stu-id="32785-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="32785-168">As ações de índice e adicionar controlador devem ser bastante familiares.</span><span class="sxs-lookup"><span data-stu-id="32785-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="32785-169">As ações do controlador remover e CartSummary lidar com dois casos especiais, que discutiremos na seção a seguir.</span><span class="sxs-lookup"><span data-stu-id="32785-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="32785-170">Atualizações de AJAX com jQuery</span><span class="sxs-lookup"><span data-stu-id="32785-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="32785-171">Em seguida, criaremos uma página de índice de carrinho de compras que terá rigidez de tipos para o ShoppingCartViewModel e usa o modelo de exibição de lista usando o mesmo método como antes.</span><span class="sxs-lookup"><span data-stu-id="32785-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="32785-172">No entanto, em vez de usar um ActionLink para remover os itens do carrinho, vamos usar jQuery "conectar" evento de clique para todos os links neste modo de exibição que têm a classe HTML RemoveLink.</span><span class="sxs-lookup"><span data-stu-id="32785-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="32785-173">Em vez de enviar o formulário, esse manipulador de eventos de clique apenas fará um retorno de chamada AJAX para nossa ação do controlador RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="32785-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="32785-174">O RemoveFromCart retorna um resultado JSON serializado, que nosso retorno de chamada do jQuery, em seguida, analisa e executa quatro atualizações rápidas para a página usando jQuery:</span><span class="sxs-lookup"><span data-stu-id="32785-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="32785-175">Remove o álbum excluído da lista</span><span class="sxs-lookup"><span data-stu-id="32785-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="32785-176">Atualiza a contagem do carrinho no cabeçalho</span><span class="sxs-lookup"><span data-stu-id="32785-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="32785-177">Exibe uma mensagem de atualização para o usuário</span><span class="sxs-lookup"><span data-stu-id="32785-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="32785-178">Atualiza o preço total do carrinho</span><span class="sxs-lookup"><span data-stu-id="32785-178">Updates the cart total price</span></span>

<span data-ttu-id="32785-179">Desde que o cenário de remoção está sendo tratado por um retorno de chamada Ajax na exibição de índice, não é necessário um modo de exibição adicional para a ação de RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="32785-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="32785-180">Aqui está o código completo para o modo de exibição de /ShoppingCart/Index:</span><span class="sxs-lookup"><span data-stu-id="32785-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="32785-181">Para testar isso, precisamos adicionar itens ao nosso carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="32785-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="32785-182">Vamos atualizar nossos **detalhes do repositório** exibição incluir um botão "Adicionar ao carrinho".</span><span class="sxs-lookup"><span data-stu-id="32785-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="32785-183">Enquanto isso, podemos pode incluir algumas informações adicionais álbum que adicionamos desde que foi atualizado pela última este modo de exibição: gênero, artista, preço e arte.</span><span class="sxs-lookup"><span data-stu-id="32785-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="32785-184">O código de exibição de detalhes do repositório atualizado é exibido conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="32785-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="32785-185">Agora podemos clique por meio da loja e adicionando e removendo álbuns de nosso carrinho de compras e de teste.</span><span class="sxs-lookup"><span data-stu-id="32785-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="32785-186">Execute o aplicativo e navegue até o índice de repositório.</span><span class="sxs-lookup"><span data-stu-id="32785-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="32785-187">Em seguida, clique em um gênero para ver uma lista de álbuns.</span><span class="sxs-lookup"><span data-stu-id="32785-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="32785-188">Clicando em um título de álbum agora mostra o modo de exibição nosso álbum detalhes atualizado, incluindo o botão "Adicionar ao carrinho".</span><span class="sxs-lookup"><span data-stu-id="32785-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="32785-189">Clicando no botão "Adicionar ao carrinho" mostra a exibição nosso índice de carrinho de compras com a lista de resumo de carrinho compras.</span><span class="sxs-lookup"><span data-stu-id="32785-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="32785-190">Depois de carregar o carrinho de compras, você pode clicar em Remover do link de carrinho para ver a atualização do Ajax ao carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="32785-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="32785-191">Criamos um trabalho carrinho que permite que os usuários não registrados adicionar itens ao carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="32785-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="32785-192">Na seção a seguir, será permitir que eles possam registrar e concluir o processo de check-out.</span><span class="sxs-lookup"><span data-stu-id="32785-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="32785-193">[Anterior](mvc-music-store-part-7.md)
> [Próximo](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="32785-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>

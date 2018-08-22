---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Parte 9: Registro e check-out | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 9 aborda o registro e check-out.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: b3babf1d935774b0ef93d6ab02c8b295998f8afc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830441"
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="3b6e5-104">Parte 9: Registro e check-out</span><span class="sxs-lookup"><span data-stu-id="3b6e5-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="3b6e5-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="3b6e5-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="3b6e5-106">A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="3b6e5-107">A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="3b6e5-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="3b6e5-109">Parte 9 aborda o registro e check-out.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="3b6e5-110">Nesta seção, criaremos um CheckoutController que coletará informações de pagamento e endereço do comprador.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="3b6e5-111">Será exigido que os usuários registrem-se em nosso site antes de fazer check-out, portanto, esse controlador exigirá autorização.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="3b6e5-112">Os usuários navegará para o processo de check-out do carrinho de compras, clicando no botão "Check-out".</span><span class="sxs-lookup"><span data-stu-id="3b6e5-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="3b6e5-113">Se o usuário não estiver conectado, ele serão solicitados a.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="3b6e5-114">Após o logon bem-sucedido, o usuário, em seguida, é mostrado o modo de exibição de endereço e o pagamento.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="3b6e5-115">Depois que eles tem preenchido o formulário e enviar o pedido, eles serão mostrados a tela de confirmação do pedido.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="3b6e5-116">A tentativa de exibir uma ordem não existente ou um pedido que não pertence a você mostrará a exibição de erro.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="3b6e5-117">Migrar o carrinho de compras</span><span class="sxs-lookup"><span data-stu-id="3b6e5-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="3b6e5-118">Enquanto o processo de compra é anônimo, quando o usuário clica no botão de check-out, serão solicitados a registrar e faça logon.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="3b6e5-119">Os usuários esperam que vamos manter suas informações do carrinho de compras entre visitas, portanto, precisamos de informações do carrinho de compras associar um usuário quando concluírem o registro ou logon.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="3b6e5-120">Isso é realmente muito simple de fazer, como a nossa classe ShoppingCart já tem um método que será associada a todos os itens no carrinho atual um nome de usuário.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="3b6e5-121">Apenas precisamos chamar esse método quando o usuário concluir o registro ou logon.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="3b6e5-122">Abra o **AccountController** classe que adicionamos ao foram podemos configurar associação e a autorização.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="3b6e5-123">Adicione um usando a instrução que faz referência MvcMusicStore.Models, em seguida, adicione o seguinte método MigrateShoppingCart:</span><span class="sxs-lookup"><span data-stu-id="3b6e5-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="3b6e5-124">Em seguida, modifique a ação de postagem de LogOn para chamar MigrateShoppingCart depois que o usuário tiver sido validado, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="3b6e5-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="3b6e5-125">Fazer com que a mesma alteração para o registro de postagem de ação, imediatamente depois que a conta de usuário é criada com êxito:</span><span class="sxs-lookup"><span data-stu-id="3b6e5-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="3b6e5-126">Isso é tudo - agora um carrinho de compras anônimo serão automaticamente transferido para uma conta de usuário após o registro foi bem-sucedido ou logon.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="3b6e5-127">Criando o CheckoutController</span><span class="sxs-lookup"><span data-stu-id="3b6e5-127">Creating the CheckoutController</span></span>

<span data-ttu-id="3b6e5-128">Clique com botão direito na pasta Controllers e adicionar um novo controlador ao projeto nomeado CheckoutController usando o modelo de controlador vazia.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="3b6e5-129">Primeiro, adicione o atributo Authorize acima da declaração de classe de controlador para exigir que os usuários registrem antes do check-out:</span><span class="sxs-lookup"><span data-stu-id="3b6e5-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="3b6e5-130">*Observação: Isso é semelhante à alteração que fizemos anteriormente para o StoreManagerController, mas nesse caso, o atributo Authorize necessário que o usuário seja em uma função de administrador. No controlador de check-out, podemos está exigindo que o usuário estar conectado, mas não exigir que eles sejam administradores.*</span><span class="sxs-lookup"><span data-stu-id="3b6e5-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="3b6e5-131">Para simplificar, estamos não lidar com informações de pagamento neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="3b6e5-132">Em vez disso, estamos permitindo que os usuários façam check-out usando um código promocional.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="3b6e5-133">Armazenaremos esse código promocional usando uma constante chamada código promocional.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="3b6e5-134">Como no StoreController, podemos declarar um campo para armazenar uma instância da classe MusicStoreEntities, chamada storeDB.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="3b6e5-135">Para fazer uso da classe MusicStoreEntities, precisamos adicionar uma instrução para o namespace MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="3b6e5-136">A parte superior do nosso controlador de check-out é exibida abaixo.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="3b6e5-137">O CheckoutController terá as seguintes ações do controlador:</span><span class="sxs-lookup"><span data-stu-id="3b6e5-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="3b6e5-138">**AddressAndPayment (método GET)** exibirá um formulário para permitir que o usuário insira suas informações.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="3b6e5-139">**AddressAndPayment (método POST)** validará a entrada e processar o pedido.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="3b6e5-140">**Completa** será mostrada após um usuário foi concluído com êxito o processo de check-out.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="3b6e5-141">Este modo de exibição incluirá o número do pedido do usuário, como confirmação.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="3b6e5-142">Primeiro, vamos renomear a ação do controlador de índice (que foi gerada quando criamos o controlador) para AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="3b6e5-143">Esta ação do controlador apenas exibe o formulário de check-out, portanto, ele não requer qualquer informação de modelo.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="3b6e5-144">Nosso método POST AddressAndPayment seguirá o mesmo padrão que usamos o StoreManagerController: ele tentará aceitar o envio do formulário e concluir o pedido e exibirá novamente o formulário se ele falhar.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="3b6e5-145">Depois de validar a entrada de formulário atende aos nossos requisitos de validação para um pedido, serão verificamos o valor de formulário de código promocional diretamente.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="3b6e5-146">Supondo que tudo está correto, vamos salvar as informações atualizadas com a ordem, informar ao objeto ShoppingCart para concluir o processo de pedido e redirecionar para a ação de conclusão.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="3b6e5-147">Após a conclusão bem-sucedida do processo de check-out, os usuários serão redirecionados para a ação do controlador completo.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="3b6e5-148">Essa ação executará uma verificação simple para validar que a ordem realmente pertencem ao usuário conectado antes de mostrar o número do pedido como uma confirmação.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="3b6e5-149">*Observação: A exibição de erro foi criada automaticamente para nós na pasta /Views/Shared quando começamos o projeto.*</span><span class="sxs-lookup"><span data-stu-id="3b6e5-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="3b6e5-150">O código completo do CheckoutController é da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="3b6e5-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="3b6e5-151">Adicionando o modo de exibição AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="3b6e5-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="3b6e5-152">Agora, vamos criar o modo de exibição AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="3b6e5-153">Clique duas vezes em uma das ações de controlador AddressAndPayment e adicione uma exibição nomeada AddressAndPayment que é fortemente tipada como um pedido e usa o modelo de edição, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="3b6e5-154">Este modo de exibição fará com que usam duas das técnicas que vimos durante a criação de exibição de StoreManagerEdit:</span><span class="sxs-lookup"><span data-stu-id="3b6e5-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="3b6e5-155">Nós usaremos Html.EditorForModel() para exibir campos de formulário para o modelo de ordem</span><span class="sxs-lookup"><span data-stu-id="3b6e5-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="3b6e5-156">Aproveitaremos usando uma classe de ordem com atributos de validação de regras de validação</span><span class="sxs-lookup"><span data-stu-id="3b6e5-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="3b6e5-157">Vamos começar atualizando o código de formulário para usar Html.EditorForModel(), seguido por uma caixa de texto adicional para o código promocional.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="3b6e5-158">O código completo para o modo de exibição AddressAndPayment é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="3b6e5-159">Definindo regras de validação para o pedido</span><span class="sxs-lookup"><span data-stu-id="3b6e5-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="3b6e5-160">Agora que nossa exibição é definida, definiremos as regras de validação para nosso modelo de ordem como fizemos anteriormente para o modelo de álbum.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="3b6e5-161">Clique com botão direito na pasta modelos e adicione uma classe denominada ordem.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="3b6e5-162">Além dos atributos de validação que foram usados anteriormente para o álbum, também usaremos uma expressão Regular para validar o endereço de email do usuário.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="3b6e5-163">A tentativa de enviar o formulário com ausente ou informações inválidas agora mostrará a mensagem de erro usando a validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="3b6e5-164">Okey, fizemos a maioria do trabalho pesado para o processo de check-out; Nós só temos alguns Miscelânea para concluir.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="3b6e5-165">Precisamos adicionar dois modos de exibição simples, e precisamos para cuidar da entrega das informações de carrinho durante o processo de logon.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="3b6e5-166">Adicionando a visão completa do check-out</span><span class="sxs-lookup"><span data-stu-id="3b6e5-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="3b6e5-167">A visão completa do check-out é muito simple, porque ele só precisa exibir a ID de ordem.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="3b6e5-168">Clique com botão direito na ação de controlador completo e adicionar uma exibição nomeada concluir que é fortemente tipada como um int.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="3b6e5-169">Agora, atualizaremos o código de exibição para exibir a ID de ordem, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="3b6e5-170">Atualizando o erro exibição</span><span class="sxs-lookup"><span data-stu-id="3b6e5-170">Updating The Error view</span></span>

<span data-ttu-id="3b6e5-171">O modelo padrão inclui uma exibição de erro na pasta de exibições de compartilhado para que ele pode ser reutilizado em outro lugar no site.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="3b6e5-172">Este modo de exibição de erro contém um erro muito simple e não usa o nosso site de Layout, portanto, iremos atualizá-lo.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="3b6e5-173">Como essa é uma página de erro genérica, o conteúdo é muito simple.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="3b6e5-174">Incluiremos uma mensagem e um link para navegar até a página anterior no histórico se o usuário deseja tentar novamente sua ação.</span><span class="sxs-lookup"><span data-stu-id="3b6e5-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> <span data-ttu-id="3b6e5-175">[Anterior](mvc-music-store-part-8.md)
> [Próximo](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="3b6e5-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>

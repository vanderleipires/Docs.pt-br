---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Parte 9: Check-out e o registro | Microsoft Docs'
author: jongalloway
description: "Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 9 abrange o registro e o check-out."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 1caf836f8c92cbc9ab95e0aa990f81493e577a27
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/12/2018
---
<a name="part-9-registration-and-checkout"></a><span data-ttu-id="f8cc3-104">Parte 9: Check-out e o registro</span><span class="sxs-lookup"><span data-stu-id="f8cc3-104">Part 9: Registration and Checkout</span></span>
====================
<span data-ttu-id="f8cc3-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="f8cc3-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="f8cc3-106">O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="f8cc3-107">O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="f8cc3-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="f8cc3-109">Parte 9 abrange o registro e o check-out.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-109">Part 9 covers Registration and Checkout.</span></span>


<span data-ttu-id="f8cc3-110">Nesta seção, estamos criando uma CheckoutController que coletará informações de pagamento e endereço do comprador.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-110">In this section, we will be creating a CheckoutController which will collect the shopper's address and payment information.</span></span> <span data-ttu-id="f8cc3-111">Podemos exigirá que os usuários registrem-se em nosso site antes de fazer check-out, para que esse controlador exigirá autorização.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-111">We will require users to register with our site prior to checking out, so this controller will require authorization.</span></span>

<span data-ttu-id="f8cc3-112">Os usuários navegará para o processo de check-out do seu carrinho de compras clicando no botão "Check-out".</span><span class="sxs-lookup"><span data-stu-id="f8cc3-112">Users will navigate to the checkout process from their shopping cart by clicking the "Checkout" button.</span></span>

![](mvc-music-store-part-9/_static/image1.jpg)

<span data-ttu-id="f8cc3-113">Se o usuário não estiver conectado, ele serão solicitado a.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-113">If the user is not logged in, they will be prompted to.</span></span>

![](mvc-music-store-part-9/_static/image1.png)

<span data-ttu-id="f8cc3-114">Após o logon bem-sucedido, o usuário, em seguida, é mostrado a exibição de endereço e pagamento.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-114">Upon successful login, the user is then shown the Address and Payment view.</span></span>

![](mvc-music-store-part-9/_static/image2.png)

<span data-ttu-id="f8cc3-115">Depois que tem preenchido o formulário e enviar o pedido, elas serão mostradas a tela de confirmação de ordem.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-115">Once they have filled the form and submitted the order, they will be shown the order confirmation screen.</span></span>

![](mvc-music-store-part-9/_static/image3.png)

<span data-ttu-id="f8cc3-116">A tentativa de exibir uma ordem não existente ou um pedido que não pertence a você mostrará a exibição de erro.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-116">Attempting to view either a non-existent order or an order that doesn't belong to you will show the Error view.</span></span>

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a><span data-ttu-id="f8cc3-117">Migrando o carrinho de compras</span><span class="sxs-lookup"><span data-stu-id="f8cc3-117">Migrating the Shopping Cart</span></span>

<span data-ttu-id="f8cc3-118">Enquanto o processo de compra é anônimo, quando o usuário clica no botão de check-out, elas serão necessárias para registrar e logon.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-118">While the shopping process is anonymous, when the user clicks on the Checkout button, they will be required to register and login.</span></span> <span data-ttu-id="f8cc3-119">Os usuários esperam que manteremos suas informações do carrinho de compras entre visitas, portanto, precisamos de informações do carrinho de compras associar um usuário quando ele concluir o registro ou logon.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-119">Users will expect that we will maintain their shopping cart information between visits, so we will need to associate the shopping cart information with a user when they complete registration or login.</span></span>

<span data-ttu-id="f8cc3-120">Isso é realmente muito simple, conforme nossa classe ShoppingCart já tem um método que vai associar todos os itens no carrinho atual com um nome de usuário.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-120">This is actually very simple to do, as our ShoppingCart class already has a method which will associate all the items in the current cart with a username.</span></span> <span data-ttu-id="f8cc3-121">Apenas precisamos chamar este método quando o usuário concluir o registro ou logon.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-121">We will just need to call this method when a user completes registration or login.</span></span>

<span data-ttu-id="f8cc3-122">Abra o **AccountController** classe adicionamos quando foram estamos configurando associação e a autorização.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-122">Open the **AccountController** class that we added when we were setting up Membership and Authorization.</span></span> <span data-ttu-id="f8cc3-123">Adicionar um usando a instrução faz referência a MvcMusicStore.Models, em seguida, adicione o seguinte método MigrateShoppingCart:</span><span class="sxs-lookup"><span data-stu-id="f8cc3-123">Add a using statement referencing MvcMusicStore.Models, then add the following MigrateShoppingCart method:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

<span data-ttu-id="f8cc3-124">Em seguida, modifique a ação de postagem de LogOn para chamar MigrateShoppingCart depois que o usuário é validado, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="f8cc3-124">Next, modify the LogOn post action to call MigrateShoppingCart after the user has been validated, as shown below:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

<span data-ttu-id="f8cc3-125">Fazer a mesma alteração para o registro após a ação, imediatamente depois que a conta de usuário é criada com êxito:</span><span class="sxs-lookup"><span data-stu-id="f8cc3-125">Make the same change to the Register post action, immediately after the user account is successfully created:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

<span data-ttu-id="f8cc3-126">-- Que é agora um carrinho de compras anônimo serão automaticamente transferido para uma conta de usuário após o registro bem-sucedido ou logon.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-126">That's it - now an anonymous shopping cart will be automatically transferred to a user account upon successful registration or login.</span></span>

## <a name="creating-the-checkoutcontroller"></a><span data-ttu-id="f8cc3-127">Criando o CheckoutController</span><span class="sxs-lookup"><span data-stu-id="f8cc3-127">Creating the CheckoutController</span></span>

<span data-ttu-id="f8cc3-128">Com o botão direito na pasta controladores e adicionar um novo controlador para o projeto chamado CheckoutController usando o modelo de controlador vazio.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-128">Right-click on the Controllers folder and add a new Controller to the project named CheckoutController using the Empty controller template.</span></span>

![](mvc-music-store-part-9/_static/image5.png)

<span data-ttu-id="f8cc3-129">Primeiro, adicione o atributo de autorizar acima da declaração de classe do controlador para exigir que os usuários se registrem antes do check-out:</span><span class="sxs-lookup"><span data-stu-id="f8cc3-129">First, add the Authorize attribute above the Controller class declaration to require users to register before checkout:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

<span data-ttu-id="f8cc3-130">*Observação: Isso é semelhante para a alteração que são feitas anteriormente para o StoreManagerController, mas nesse caso o atributo de autorização necessário que o usuário seja em uma função de administrador. No controlador de check-out, está exigindo o usuário conectado, mas não exigir que eles sejam administradores.*</span><span class="sxs-lookup"><span data-stu-id="f8cc3-130">*Note: This is similar to the change we previously made to the StoreManagerController, but in that case the Authorize attribute required that the user be in an Administrator role. In the Checkout Controller, we're requiring the user be logged in but aren't requiring that they be administrators.*</span></span>

<span data-ttu-id="f8cc3-131">Para simplificar, nós não lidar com informações de pagamento neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-131">For the sake of simplicity, we won't be dealing with payment information in this tutorial.</span></span> <span data-ttu-id="f8cc3-132">Em vez disso, podemos está permitindo que os usuários façam check-out usando um código promocional.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-132">Instead, we are allowing users to check out using a promotional code.</span></span> <span data-ttu-id="f8cc3-133">Armazenaremos esse código promocional usando uma constante chamada PromoCode.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-133">We will store this promotional code using a constant named PromoCode.</span></span>

<span data-ttu-id="f8cc3-134">Como StoreController, podemos irá declarar um campo para manter uma instância da classe MusicStoreEntities, denominado storeDB.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-134">As in the StoreController, we'll declare a field to hold an instance of the MusicStoreEntities class, named storeDB.</span></span> <span data-ttu-id="f8cc3-135">Para fazer uso da classe MusicStoreEntities, precisamos adicionar um usando a instrução do namespace MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-135">In order to make use of the MusicStoreEntities class, we will need to add a using statement for the MvcMusicStore.Models namespace.</span></span> <span data-ttu-id="f8cc3-136">A parte superior do nosso controlador check-out será exibido abaixo.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-136">The top of our Checkout controller appears below.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

<span data-ttu-id="f8cc3-137">O CheckoutController terá as seguintes ações do controlador:</span><span class="sxs-lookup"><span data-stu-id="f8cc3-137">The CheckoutController will have the following controller actions:</span></span>

<span data-ttu-id="f8cc3-138">**AddressAndPayment (método GET)** exibirá um formulário para permitir que o usuário insira suas informações.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-138">**AddressAndPayment (GET method)** will display a form to allow the user to enter their information.</span></span>

<span data-ttu-id="f8cc3-139">**AddressAndPayment (método POST)** será validar a entrada e processar o pedido.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-139">**AddressAndPayment (POST method)** will validate the input and process the order.</span></span>

<span data-ttu-id="f8cc3-140">**Concluída** será mostrado após um usuário concluído com êxito o processo de check-out.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-140">**Complete** will be shown after a user has successfully finished the checkout process.</span></span> <span data-ttu-id="f8cc3-141">Este modo de exibição incluirá o número do pedido do usuário, como confirmação.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-141">This view will include the user's order number, as confirmation.</span></span>

<span data-ttu-id="f8cc3-142">Primeiro, vamos renomear a ação de controlador de índice (que foi gerada quando criamos o controlador) para AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-142">First, let's rename the Index controller action (which was generated when we created the controller) to AddressAndPayment.</span></span> <span data-ttu-id="f8cc3-143">Esta ação de controlador apenas exibe o formulário de check-out, portanto ele não requer qualquer informação de modelo.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-143">This controller action just displays the checkout form, so it doesn't require any model information.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

<span data-ttu-id="f8cc3-144">Nosso método POST AddressAndPayment seguirão o mesmo padrão que usamos o StoreManagerController: ele tentará aceitar o envio do formulário e concluir o pedido e exibirá novamente o formulário se ele falhar.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-144">Our AddressAndPayment POST method will follow the same pattern we used in the StoreManagerController: it will try to accept the form submission and complete the order, and will re-display the form if it fails.</span></span>

<span data-ttu-id="f8cc3-145">Após validar a entrada de formulário atende os requisitos de validação para uma ordem, podemos verificará o valor de formulário PromoCode diretamente.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-145">After validating the form input meets our validation requirements for an Order, we will check the PromoCode form value directly.</span></span> <span data-ttu-id="f8cc3-146">Se que tudo estiver correto, que podemos salvará as informações atualizadas com a ordem, informe o objeto ShoppingCart para concluir o processo e redirecionar para a ação.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-146">Assuming everything is correct, we will save the updated information with the order, tell the ShoppingCart object to complete the order process, and redirect to the Complete action.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

<span data-ttu-id="f8cc3-147">Após a conclusão bem-sucedida do processo de check-out, os usuários serão redirecionados para a ação de controlador concluída.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-147">Upon successful completion of the checkout process, users will be redirected to the Complete controller action.</span></span> <span data-ttu-id="f8cc3-148">Esta ação executará uma verificação simple para validar a ordem realmente pertencem ao usuário conectado antes de mostrar o número do pedido como uma confirmação.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-148">This action will perform a simple check to validate that the order does indeed belong to the logged-in user before showing the order number as a confirmation.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

<span data-ttu-id="f8cc3-149">*Observação: O modo de exibição de erro foi criado automaticamente para que possamos na pasta /Views/Shared quando começamos o projeto.*</span><span class="sxs-lookup"><span data-stu-id="f8cc3-149">*Note: The Error view was automatically created for us in the /Views/Shared folder when we began the project.*</span></span>

<span data-ttu-id="f8cc3-150">O código completo de CheckoutController é o seguinte:</span><span class="sxs-lookup"><span data-stu-id="f8cc3-150">The complete CheckoutController code is as follows:</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a><span data-ttu-id="f8cc3-151">Adicionando o modo de exibição AddressAndPayment</span><span class="sxs-lookup"><span data-stu-id="f8cc3-151">Adding the AddressAndPayment view</span></span>

<span data-ttu-id="f8cc3-152">Agora, vamos criar o modo de exibição AddressAndPayment.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-152">Now, let's create the AddressAndPayment view.</span></span> <span data-ttu-id="f8cc3-153">Clique duas vezes em uma das ações de controlador AddressAndPayment e adicione uma exibição nomeada AddressAndPayment é fortemente tipada como uma ordem e que usa o modelo de edição, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-153">Right-click on one of the AddressAndPayment controller actions and add a view named AddressAndPayment which is strongly typed as an Order and uses the Edit template, as shown below.</span></span>

![](mvc-music-store-part-9/_static/image6.png)

<span data-ttu-id="f8cc3-154">Este modo de exibição fará com que o uso de duas das técnicas examinamos ao criar o modo de exibição de StoreManagerEdit:</span><span class="sxs-lookup"><span data-stu-id="f8cc3-154">This view will make use of two of the techniques we looked at while building the StoreManagerEdit view:</span></span>

- <span data-ttu-id="f8cc3-155">Usaremos Html.EditorForModel() para exibir campos de formulário para o modelo de ordem</span><span class="sxs-lookup"><span data-stu-id="f8cc3-155">We will use Html.EditorForModel() to display form fields for the Order model</span></span>
- <span data-ttu-id="f8cc3-156">Estenderemos usando uma classe de ordem com atributos de validação de regras de validação</span><span class="sxs-lookup"><span data-stu-id="f8cc3-156">We will leverage validation rules using an Order class with validation attributes</span></span>

<span data-ttu-id="f8cc3-157">Vamos começar atualizando o código de formulário para usar Html.EditorForModel(), seguido por uma caixa de texto adicional para o código de promoção.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-157">We'll start by updating the form code to use Html.EditorForModel(), followed by an additional textbox for the Promo Code.</span></span> <span data-ttu-id="f8cc3-158">O código completo para o modo de exibição de AddressAndPayment é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-158">The complete code for the AddressAndPayment view is shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a><span data-ttu-id="f8cc3-159">Definir regras de validação para o pedido</span><span class="sxs-lookup"><span data-stu-id="f8cc3-159">Defining validation rules for the Order</span></span>

<span data-ttu-id="f8cc3-160">Agora que nossa exibição estiver definida, podemos configurará as regras de validação para nosso modelo de ordem como fizemos anteriormente para o modelo de álbum.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-160">Now that our view is set up, we will set up the validation rules for our Order model as we did previously for the Album model.</span></span> <span data-ttu-id="f8cc3-161">Com o botão direito na pasta modelos e adicione uma classe denominada ordem.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-161">Right-click on the Models folder and add a class named Order.</span></span> <span data-ttu-id="f8cc3-162">Além dos atributos de validação que foram usados anteriormente para o álbum, também usaremos uma expressão Regular para validar o endereço de email do usuário.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-162">In addition to the validation attributes we used previously for the Album, we will also be using a Regular Expression to validate the user's email address.</span></span>

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

<span data-ttu-id="f8cc3-163">A tentativa de enviar o formulário com ausente ou inválidas informações agora mostrará a mensagem de erro usando a validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-163">Attempting to submit the form with missing or invalid information will now show error message using client-side validation.</span></span>

![](mvc-music-store-part-9/_static/image7.png)

<span data-ttu-id="f8cc3-164">Fizemos Okey, a maioria do trabalho de disco rígido para o processo de check-out. Assim, temos alguns termina e de probabilidade para concluir.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-164">Okay, we've done most of the hard work for the checkout process; we just have a few odds and ends to finish.</span></span> <span data-ttu-id="f8cc3-165">Precisamos adicionar dois modos de exibição simples e é necessário cuidar da entrega as informações do carrinho durante o processo de logon.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-165">We need to add two simple views, and we need to take care of the handoff of the cart information during the login process.</span></span>

## <a name="adding-the-checkout-complete-view"></a><span data-ttu-id="f8cc3-166">Adicionando a exibição completa de check-out</span><span class="sxs-lookup"><span data-stu-id="f8cc3-166">Adding the Checkout Complete view</span></span>

<span data-ttu-id="f8cc3-167">A exibição completa de check-out é muito simple, como ele só precisa exibir a ID da ordem.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-167">The Checkout Complete view is pretty simple, as it just needs to display the Order ID.</span></span> <span data-ttu-id="f8cc3-168">Clique na ação do controlador completa e adicionar uma exibição nomeada concluir que é fortemente tipada como int.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-168">Right-click on the Complete controller action and add a view named Complete which is strongly typed as an int.</span></span>

![](mvc-music-store-part-9/_static/image8.png)

<span data-ttu-id="f8cc3-169">Agora atualizaremos o código de exibição para exibir a ID da ordem, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-169">Now we will update the view code to display the Order ID, as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a><span data-ttu-id="f8cc3-170">Atualizando o erro exibição</span><span class="sxs-lookup"><span data-stu-id="f8cc3-170">Updating The Error view</span></span>

<span data-ttu-id="f8cc3-171">O modelo padrão inclui uma exibição de erro na pasta compartilhada de modos de exibição para que pode ser reutilizada em outro lugar no site.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-171">The default template includes an Error view in the Shared views folder so that it can be re-used elsewhere in the site.</span></span> <span data-ttu-id="f8cc3-172">Este modo de exibição de erro contém um erro muito simple e não usa o nosso site de Layout, portanto vamos atualizá-lo.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-172">This Error view contains a very simple error and doesn't use our site Layout, so we'll update it.</span></span>

<span data-ttu-id="f8cc3-173">Como esta é uma página de erro genérica, o conteúdo é muito simple.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-173">Since this is a generic error page, the content is very simple.</span></span> <span data-ttu-id="f8cc3-174">Podemos incluirá uma mensagem e um link para navegar até a página anterior no histórico se o usuário deseja tentar novamente a ação.</span><span class="sxs-lookup"><span data-stu-id="f8cc3-174">We'll include a message and a link to navigate to the previous page in history if the user wants to re-try their action.</span></span>

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


>[!div class="step-by-step"]
<span data-ttu-id="f8cc3-175">[Anterior](mvc-music-store-part-8.md)
[Próximo](mvc-music-store-part-10.md)</span><span class="sxs-lookup"><span data-stu-id="f8cc3-175">[Previous](mvc-music-store-part-8.md)
[Next](mvc-music-store-part-10.md)</span></span>

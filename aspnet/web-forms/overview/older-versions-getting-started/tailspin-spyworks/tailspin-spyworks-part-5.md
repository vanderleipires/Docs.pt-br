---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Parte 5: Lógica de negócios | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 5 adiciona alguma lógica de negócios.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e4342e634ef8c4bcf4e0085650a28f414ab23736
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="part-5-business-logic"></a><span data-ttu-id="0c468-104">Parte 5: Lógica de negócios</span><span class="sxs-lookup"><span data-stu-id="0c468-104">Part 5: Business Logic</span></span>
====================
<span data-ttu-id="0c468-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="0c468-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="0c468-106">Tailspin Spyworks demonstra como extraordinariamente simples é criar aplicativos eficientes e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="0c468-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="0c468-107">Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, inclusive de compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="0c468-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="0c468-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="0c468-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="0c468-109">Parte 5 adiciona alguma lógica de negócios.</span><span class="sxs-lookup"><span data-stu-id="0c468-109">Part 5 adds some business logic.</span></span>


## <a id="_Toc260221671"></a>  <span data-ttu-id="0c468-110">Adicionando alguma lógica de negócios</span><span class="sxs-lookup"><span data-stu-id="0c468-110">Adding Some Business Logic</span></span>

<span data-ttu-id="0c468-111">Queremos que nossa experiência de compra estejam disponíveis sempre que alguém visitar nosso site.</span><span class="sxs-lookup"><span data-stu-id="0c468-111">We want our shopping experience to be available whenever someone visits our web site.</span></span> <span data-ttu-id="0c468-112">Os visitantes poderão procurar e adicionar itens ao carrinho de compras, mesmo se eles não são registrados ou registrados no.</span><span class="sxs-lookup"><span data-stu-id="0c468-112">Visitors will be able to browse and add items to the shopping cart even if they are not registered or logged in.</span></span> <span data-ttu-id="0c468-113">Quando estiverem prontas para check-out eles recebem a opção de autenticar e se não estiverem ainda membros poderão criar uma conta.</span><span class="sxs-lookup"><span data-stu-id="0c468-113">When they are ready to check out they will be given the option to authenticate and if they are not yet members they will be able to create an account.</span></span>

<span data-ttu-id="0c468-114">Isso significa que será necessário implementar a lógica para converter o carrinho de compras de um estado anônimo para um estado de "Usuário registrado".</span><span class="sxs-lookup"><span data-stu-id="0c468-114">This means that we will need to implement the logic to convert the shopping cart from an anonymous state to a "Registered User" state.</span></span>

<span data-ttu-id="0c468-115">Vamos criar um diretório chamado "Classes", em seguida, com o botão direito na pasta e criar um novo arquivo de "Classe" denominado MyShoppingCart.cs</span><span class="sxs-lookup"><span data-stu-id="0c468-115">Let's create a directory named "Classes" then Right-Click on the folder and create a new "Class" file named MyShoppingCart.cs</span></span>

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

<span data-ttu-id="0c468-116">Conforme mencionado anteriormente, estender a classe que implementa a página MyShoppingCart.aspx e podemos faz isso usando. Construir avançada "classe parcial" do NET.</span><span class="sxs-lookup"><span data-stu-id="0c468-116">As previously mentioned we will be extending the class that implements the MyShoppingCart.aspx page and we will do this using .NET's powerful "Partial Class" construct.</span></span>

<span data-ttu-id="0c468-117">A chamada gerada para nosso arquivo MyShoppingCart.aspx.cf tem esta aparência.</span><span class="sxs-lookup"><span data-stu-id="0c468-117">The generated call for our MyShoppingCart.aspx.cf file looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

<span data-ttu-id="0c468-118">Observe o uso da palavra-chave "parcial".</span><span class="sxs-lookup"><span data-stu-id="0c468-118">Note the use of the "partial" keyword.</span></span>

<span data-ttu-id="0c468-119">O arquivo de classe que acabou de gerar tem esta aparência.</span><span class="sxs-lookup"><span data-stu-id="0c468-119">The class file that we just generated looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

<span data-ttu-id="0c468-120">Podemos mesclará nossas implementações adicionando a palavra-chave partial para este arquivo.</span><span class="sxs-lookup"><span data-stu-id="0c468-120">We will merge our implementations by adding the partial keyword to this file as well.</span></span>

<span data-ttu-id="0c468-121">Nosso novo arquivo de classe agora tem esta aparência.</span><span class="sxs-lookup"><span data-stu-id="0c468-121">Our new class file now looks like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

<span data-ttu-id="0c468-122">O primeiro método que vamos adicionar à nossa classe é o método "AddItem".</span><span class="sxs-lookup"><span data-stu-id="0c468-122">The first method that we will add to our class is the "AddItem" method.</span></span> <span data-ttu-id="0c468-123">Esse é o método que será chamado, por fim, quando o usuário clicar nos links "Adicionar a arte" nas páginas de lista de produtos e os detalhes do produto.</span><span class="sxs-lookup"><span data-stu-id="0c468-123">This is the method that will ultimately be called when the user clicks on the "Add to Art" links on the Product List and Product Details pages.</span></span>

<span data-ttu-id="0c468-124">Acrescente o seguinte ao usando instruções na parte superior da página.</span><span class="sxs-lookup"><span data-stu-id="0c468-124">Append the following to the using statements at the top of the page.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

<span data-ttu-id="0c468-125">E adicione esse método à classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="0c468-125">And add this method to the MyShoppingCart class.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

<span data-ttu-id="0c468-126">Estamos usando LINQ to Entities para ver se o item já está no carrinho.</span><span class="sxs-lookup"><span data-stu-id="0c468-126">We are using LINQ to Entities to see if the item is already in the cart.</span></span> <span data-ttu-id="0c468-127">Se assim, podemos atualizar a quantidade da ordem do item, caso contrário, crie uma nova entrada para o item selecionado</span><span class="sxs-lookup"><span data-stu-id="0c468-127">If so, we update the order quantity of the item, otherwise we create a new entry for the selected item</span></span>

<span data-ttu-id="0c468-128">Para chamar esse método Vamos implementar uma página AddToCart.aspx não apenas esse método de classe, mas, em seguida, exibidos atual um = carrinho de compras depois que o item foi adicionado.</span><span class="sxs-lookup"><span data-stu-id="0c468-128">In order to call this method we will implement an AddToCart.aspx page that not only class this method but then displayed the current shopping a=cart after the item has been added.</span></span>

<span data-ttu-id="0c468-129">Clique com botão direito no nome da solução no Gerenciador de soluções e adicionar e nova página chamada AddToCart.aspx como fizemos anteriormente.</span><span class="sxs-lookup"><span data-stu-id="0c468-129">Right-Click on the solution name in the solution explorer and add and new page named AddToCart.aspx as we have done previously.</span></span>

<span data-ttu-id="0c468-130">Enquanto podemos usar essa página para exibir os resultados intermediários como baixos problemas de estoque, etc., em nossa implementação, a página será, na verdade, não renderizar, mas em vez disso, chame a lógica de "Adicionar" e redirecionar.</span><span class="sxs-lookup"><span data-stu-id="0c468-130">While we could use this page to display interim results like low stock issues, etc, in our implementation, the page will not actually render, but rather call the "Add" logic and redirect.</span></span>

<span data-ttu-id="0c468-131">Para fazer isso, adicionaremos o código a seguir para a página\_evento de carregamento.</span><span class="sxs-lookup"><span data-stu-id="0c468-131">To accomplish this we'll add the following code to the Page\_Load event.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

<span data-ttu-id="0c468-132">Observe que o produto para adicionar ao carrinho de compras de um parâmetro de QueryString e chamar o método AddItem de nossa classe está sendo recuperado.</span><span class="sxs-lookup"><span data-stu-id="0c468-132">Note that we are retrieving the product to add to the shopping cart from a QueryString parameter and calling the AddItem method of our class.</span></span>

<span data-ttu-id="0c468-133">Supondo que nenhum erro for encontrado controle é passado para a página de SHoppingCart.aspx que totalmente Vamos implementar em seguida.</span><span class="sxs-lookup"><span data-stu-id="0c468-133">Assuming no errors are encountered control is passed to the SHoppingCart.aspx page which we will fully implement next.</span></span> <span data-ttu-id="0c468-134">Se houver um erro, lançamos uma exceção.</span><span class="sxs-lookup"><span data-stu-id="0c468-134">If there should be an error we throw an exception.</span></span>

<span data-ttu-id="0c468-135">No momento não ainda implementamos um manipulador de erro global para passariam essa exceção não tratada pelo nosso aplicativo, mas podemos corrigirá isso em breve.</span><span class="sxs-lookup"><span data-stu-id="0c468-135">Currently we have not yet implemented a global error handler so this exception would go unhandled by our application but we will remedy this shortly.</span></span>

<span data-ttu-id="0c468-136">Observe também o uso da instrução Debug.Fail() (disponíveis por meio de `using System.Diagnostics;)`</span><span class="sxs-lookup"><span data-stu-id="0c468-136">Note also the use of the statement Debug.Fail() (available via `using System.Diagnostics;)`</span></span>

<span data-ttu-id="0c468-137">É o aplicativo é executado dentro do depurador, este método exibirá uma caixa de diálogo detalhada com informações sobre o estado de aplicativos junto com a mensagem de erro que especificamos.</span><span class="sxs-lookup"><span data-stu-id="0c468-137">Is the application is running inside the debugger, this method will display a detailed dialog with information about the applications state along with the error message that we specify.</span></span>

<span data-ttu-id="0c468-138">Quando executado em produção, a instrução Debug.Fail() será ignorado.</span><span class="sxs-lookup"><span data-stu-id="0c468-138">When running in production the Debug.Fail() statement is ignored.</span></span>

<span data-ttu-id="0c468-139">Você observará no código acima de uma chamada para um método em nossos nomes de classe carrinho compras "GetShoppingCartId".</span><span class="sxs-lookup"><span data-stu-id="0c468-139">You will note in the code above a call to a method in our shopping cart class names "GetShoppingCartId".</span></span>

<span data-ttu-id="0c468-140">Adicione o código para implementar o método da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="0c468-140">Add the code to implement the method as follows.</span></span>

<span data-ttu-id="0c468-141">Observe que também adicionamos botões de atualização e o check-out e um rótulo em que podemos exibir carrinho de "total".</span><span class="sxs-lookup"><span data-stu-id="0c468-141">Note that we've also added update and checkout buttons and a label where we can display the cart "total".</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

<span data-ttu-id="0c468-142">Agora podemos pode adicionar itens ao nosso carrinho de compras, mas não implementamos a lógica para exibir o carrinho depois da adição de um produto.</span><span class="sxs-lookup"><span data-stu-id="0c468-142">We can now add items to our shopping cart but we have not implemented the logic to display the cart after a product has been added.</span></span>

<span data-ttu-id="0c468-143">Portanto, a página MyShoppingCart.aspx adicionaremos um controle EntityDataSource e o controle GridVire da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="0c468-143">So, in the MyShoppingCart.aspx page we'll add an EntityDataSource control and a GridVire control as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

<span data-ttu-id="0c468-144">Chame o formulário no designer de forma que você pode clicar duas vezes em um botão Atualizar carrinho e gerar o manipulador de eventos de clique que é especificado na declaração na marcação.</span><span class="sxs-lookup"><span data-stu-id="0c468-144">Call up the form in the designer so that you can double click on the Update Cart button and generate the click event handler that is specified in the declaration in the markup.</span></span>

<span data-ttu-id="0c468-145">Implementaremos os detalhes mais tarde, mas isso vamos compilar e executar o nosso aplicativo sem erros.</span><span class="sxs-lookup"><span data-stu-id="0c468-145">We'll implement the details later but doing this will let us build and run our application without errors.</span></span>

<span data-ttu-id="0c468-146">Quando você executa o aplicativo e adiciona um item ao carrinho de compras, você verá isso.</span><span class="sxs-lookup"><span data-stu-id="0c468-146">When you run the application and add an item to the shopping cart you will see this.</span></span>

![](tailspin-spyworks-part-5/_static/image2.jpg)

<span data-ttu-id="0c468-147">Observe que estamos ter deviated de exibição de grade "padrão" Implementando três colunas personalizadas.</span><span class="sxs-lookup"><span data-stu-id="0c468-147">Note that we have deviated from the "default" grid display by implementing three custom columns.</span></span>

<span data-ttu-id="0c468-148">A primeira é uma editável, o campo "Associado" para a quantidade:</span><span class="sxs-lookup"><span data-stu-id="0c468-148">The first is an Editable, "Bound" field for the Quantity:</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

<span data-ttu-id="0c468-149">O próximo é uma coluna "calculada" que exibe o total de item de linha (o item de custo vezes a quantidade a ser ordenada):</span><span class="sxs-lookup"><span data-stu-id="0c468-149">The next is a "calculated" column that displays the line item total (the item cost times the quantity to be ordered):</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

<span data-ttu-id="0c468-150">Por fim, temos uma coluna personalizada que contém um controle de caixa de seleção que o usuário usará para indicar que o item deve ser removido do gráfico de compras.</span><span class="sxs-lookup"><span data-stu-id="0c468-150">Lastly we have a custom column that contains a CheckBox control that the user will use to indicate that the item should be removed from the shopping chart.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

<span data-ttu-id="0c468-151">Como você pode ver, a ordem Total da linha é vazia por isso vamos adicionar alguns lógica para calcular o Total de pedidos.</span><span class="sxs-lookup"><span data-stu-id="0c468-151">As you can see, the Order Total line is empty so let's add some logic to calculate the Order Total.</span></span>

<span data-ttu-id="0c468-152">Primeiro, implementaremos um método "GetTotal genérica" à nossa classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="0c468-152">We'll first implement a "GetTotal" method to our MyShoppingCart Class.</span></span>

<span data-ttu-id="0c468-153">No arquivo MyShoppingCart.cs, adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="0c468-153">In the MyShoppingCart.cs file add the following code.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

<span data-ttu-id="0c468-154">Em seguida, na página\_manipulador de eventos de carga pode chamaremos nosso método GetTotal genérica.</span><span class="sxs-lookup"><span data-stu-id="0c468-154">Then in the Page\_Load event handler we'll can call our GetTotal method.</span></span> <span data-ttu-id="0c468-155">Ao mesmo tempo, adicionaremos um teste para verificar se o carrinho de compras está vazio e ajustar a exibição adequadamente, se ele for.</span><span class="sxs-lookup"><span data-stu-id="0c468-155">At the same time we'll add a test to see if the shopping cart is empty and adjust the display accordingly if it is.</span></span>

<span data-ttu-id="0c468-156">Agora o carrinho de compras está vazio se isso:</span><span class="sxs-lookup"><span data-stu-id="0c468-156">Now if the shopping cart is empty we get this:</span></span>

![](tailspin-spyworks-part-5/_static/image4.jpg)

<span data-ttu-id="0c468-157">E se não, podemos ver nosso total.</span><span class="sxs-lookup"><span data-stu-id="0c468-157">And if not, we see our total.</span></span>

![](tailspin-spyworks-part-5/_static/image5.jpg)

<span data-ttu-id="0c468-158">No entanto, essa página ainda não está completa.</span><span class="sxs-lookup"><span data-stu-id="0c468-158">However, this page is not yet complete.</span></span>

<span data-ttu-id="0c468-159">Precisaremos lógica adicional para recalcular o carrinho de compras, removendo itens marcados para remoção e determinando novos valores de quantidade como alguns podem ter sido alterado na grade pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="0c468-159">We will need additional logic to recalculate the shopping cart by removing items marked for removal and by determining new quantity values as some may have been changed in the grid by the user.</span></span>

<span data-ttu-id="0c468-160">Permite adicionar um método "RemoveItem" a nossa classe de carrinho de compras em MyShoppingCart.cs para lidar com o caso quando um usuário marca um item para remoção.</span><span class="sxs-lookup"><span data-stu-id="0c468-160">Lets add a "RemoveItem" method to our shopping cart class in MyShoppingCart.cs to handle the case when a user marks an item for removal.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

<span data-ttu-id="0c468-161">Agora vamos ad um método para tratar o caso quando um usuário altera simplesmente a qualidade seja ordenado em GridView.</span><span class="sxs-lookup"><span data-stu-id="0c468-161">Now let's ad a method to handle the circumstance when a user simply changes the quality to be ordered in the GridView.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

<span data-ttu-id="0c468-162">Com os recursos básicos de remoção e atualização in-loco, podemos implementar a lógica que atualiza, na verdade, o carrinho de compras no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0c468-162">With the basic Remove and Update features in place we can implement the logic that actually updates the shopping cart in the database.</span></span> <span data-ttu-id="0c468-163">(Em MyShoppingCart.cs)</span><span class="sxs-lookup"><span data-stu-id="0c468-163">(In MyShoppingCart.cs)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

<span data-ttu-id="0c468-164">Você observará que este método espera dois parâmetros.</span><span class="sxs-lookup"><span data-stu-id="0c468-164">You'll note that this method expects two parameters.</span></span> <span data-ttu-id="0c468-165">Uma é o Id do carrinho de compras e a outra é uma matriz de objetos do tipo definido pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="0c468-165">One is the shopping cart Id and the other is an array of objects of user defined type.</span></span>

<span data-ttu-id="0c468-166">Para minimizar a dependência de nossa lógica em especificações de interface do usuário, definimos uma estrutura de dados que podemos usar para passar os itens do carrinho de compras para nosso código sem nosso método precisar acessar diretamente o controle GridView.</span><span class="sxs-lookup"><span data-stu-id="0c468-166">So as to minimize the dependency of our logic on user interface specifics, we've defined a data structure that we can use to pass the shopping cart items to our code without our method needing to directly access the GridView control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

<span data-ttu-id="0c468-167">Em nosso arquivo MyShoppingCart.aspx.cs podemos usar esta estrutura em nosso manipulador de eventos de clique de botão de atualização da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="0c468-167">In our MyShoppingCart.aspx.cs file we can use this structure in our Update Button Click Event handler as follows.</span></span> <span data-ttu-id="0c468-168">Observe que, além de atualizar o carrinho atualizaremos total carrinho.</span><span class="sxs-lookup"><span data-stu-id="0c468-168">Note that in addition to updating the cart we will update the cart total as well.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

<span data-ttu-id="0c468-169">Observe, com interesse em particular, esta linha de código:</span><span class="sxs-lookup"><span data-stu-id="0c468-169">Note with particular interest this line of code:</span></span>

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

<span data-ttu-id="0c468-170">GetValues() é uma função auxiliar especial que vamos implementar em MyShoppingCart.aspx.cs da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="0c468-170">GetValues() is a special helper function that we will implement in MyShoppingCart.aspx.cs as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

<span data-ttu-id="0c468-171">Isso fornece uma maneira simples para acessar os valores dos elementos associados no nosso controle GridView.</span><span class="sxs-lookup"><span data-stu-id="0c468-171">This provides a clean way to access the values of the bound elements in our GridView control.</span></span> <span data-ttu-id="0c468-172">Como nosso controle de caixa de seleção "Remover o Item" não está ligado vamos acessá-lo por meio do método FindControl().</span><span class="sxs-lookup"><span data-stu-id="0c468-172">Since our "Remove Item" CheckBox Control is not bound we'll access it via the FindControl() method.</span></span>

<span data-ttu-id="0c468-173">Neste estágio do desenvolvimento do seu projeto estamos obtendo prontos para implementar o processo de check-out.</span><span class="sxs-lookup"><span data-stu-id="0c468-173">At this stage in your project's development we are getting ready to implement the checkout process.</span></span>

<span data-ttu-id="0c468-174">Antes de fazer isso permite use o Visual Studio para gerar o banco de dados de associação e adicionar um usuário para o repositório de associação.</span><span class="sxs-lookup"><span data-stu-id="0c468-174">Before doing so let's use Visual Studio to generate the membership database and add a user to the membership repository.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0c468-175">[Anterior](tailspin-spyworks-part-4.md)
> [Próximo](tailspin-spyworks-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="0c468-175">[Previous](tailspin-spyworks-part-4.md)
[Next](tailspin-spyworks-part-6.md)</span></span>

---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Parte 7: Adicionando recursos | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 7 adiciona recursos adicionais, como a conta revie...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: cada8d9aee649e4f2a5afc1ca2b46863ea458207
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833041"
---
<a name="part-7-adding-features"></a><span data-ttu-id="63ed9-104">Parte 7: Adição de recursos</span><span class="sxs-lookup"><span data-stu-id="63ed9-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="63ed9-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="63ed9-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="63ed9-106">Tailspin Spyworks demonstra como incrivelmente simples é criar aplicativos avançados e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="63ed9-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="63ed9-107">Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, incluindo as compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="63ed9-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="63ed9-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="63ed9-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="63ed9-109">Parte 7 adiciona recursos adicionais, como análise de conta, análises de produtos e "itens populares" e controles de usuário "também adquiridas".</span><span class="sxs-lookup"><span data-stu-id="63ed9-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a>  <span data-ttu-id="63ed9-110">Adição de recursos</span><span class="sxs-lookup"><span data-stu-id="63ed9-110">Adding Features</span></span>

<span data-ttu-id="63ed9-111">Embora os usuários podem procurar nosso catálogo, colocar itens no carrinho de compras e concluir o processo de check-out, há que um número de dar suporte a recursos que será incluído para melhorar nosso site.</span><span class="sxs-lookup"><span data-stu-id="63ed9-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="63ed9-112">Revisão de conta (lista de pedidos colocados e exibir detalhes.)</span><span class="sxs-lookup"><span data-stu-id="63ed9-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="63ed9-113">Adicione algum conteúdo específico do contexto para a página inicial.</span><span class="sxs-lookup"><span data-stu-id="63ed9-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="63ed9-114">Adicione um recurso para permitir que os usuários de revisão de produtos no catálogo.</span><span class="sxs-lookup"><span data-stu-id="63ed9-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="63ed9-115">Crie um controle de usuário para exibir itens populares e lugar que controlam na página inicial.</span><span class="sxs-lookup"><span data-stu-id="63ed9-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="63ed9-116">Criar um controle de usuário "Também adquiridos" e adicione-o para a página de detalhes do produto.</span><span class="sxs-lookup"><span data-stu-id="63ed9-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="63ed9-117">Adicionar um contato de página.</span><span class="sxs-lookup"><span data-stu-id="63ed9-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="63ed9-118">Adicionar uma página sobre.</span><span class="sxs-lookup"><span data-stu-id="63ed9-118">Add an About Page.</span></span>
8. <span data-ttu-id="63ed9-119">Erro global</span><span class="sxs-lookup"><span data-stu-id="63ed9-119">Global Error</span></span>

## <a id="_Toc260221674"></a>  <span data-ttu-id="63ed9-120">Revisão de conta</span><span class="sxs-lookup"><span data-stu-id="63ed9-120">Account Review</span></span>

<span data-ttu-id="63ed9-121">Na pasta "Conta", crie duas páginas. aspx um OrderList.aspx nomeado e a outros OrderDetails.aspx nomeado</span><span class="sxs-lookup"><span data-stu-id="63ed9-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="63ed9-122">OrderList.aspx aproveitará os controles GridView e EntityDataSoure quanto temos anteriormente.</span><span class="sxs-lookup"><span data-stu-id="63ed9-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="63ed9-123">O EntityDataSoure seleciona os registros da tabela filtrada no nome de usuário (consulte o WhereParameter) que definimos em uma variável de sessão quando o log do usuário do dentro.</span><span class="sxs-lookup"><span data-stu-id="63ed9-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="63ed9-124">Observe também esses parâmetros no HyperlinkField de GridView:</span><span class="sxs-lookup"><span data-stu-id="63ed9-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="63ed9-125">Eles especificam o link para a exibição de detalhes do pedido para cada produto especificando o campo de OrderID como um parâmetro QueryString para a página OrderDetails.aspx.</span><span class="sxs-lookup"><span data-stu-id="63ed9-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a>  <span data-ttu-id="63ed9-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="63ed9-126">OrderDetails.aspx</span></span>

<span data-ttu-id="63ed9-127">Usaremos um controle EntityDataSource para acessar os pedidos e um FormView para exibir os dados de pedidos e outro EntityDataSource com um GridView para exibir todos os itens da ordem de linha.</span><span class="sxs-lookup"><span data-stu-id="63ed9-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="63ed9-128">No arquivo Code-Behind (OrderDetails.aspx.cs), temos dois bits pouco de manutenção do sistema.</span><span class="sxs-lookup"><span data-stu-id="63ed9-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="63ed9-129">Primeiro, precisamos garantir que OrderDetails sempre obtém um OrderId.</span><span class="sxs-lookup"><span data-stu-id="63ed9-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="63ed9-130">Também precisamos calcular e exibir o total de itens de linha do pedido.</span><span class="sxs-lookup"><span data-stu-id="63ed9-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  <span data-ttu-id="63ed9-131">A Home Page</span><span class="sxs-lookup"><span data-stu-id="63ed9-131">The Home Page</span></span>

<span data-ttu-id="63ed9-132">Vamos adicionar algum conteúdo estático para a página Default. aspx.</span><span class="sxs-lookup"><span data-stu-id="63ed9-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="63ed9-133">Primeiro vou criar uma pasta de "Conteúdo" e dentro de uma pasta de imagens (e vou incluir uma imagem a ser usada na home page).</span><span class="sxs-lookup"><span data-stu-id="63ed9-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="63ed9-134">Para o espaço reservado de parte inferior da página Default. aspx, adicione a seguinte marcação.</span><span class="sxs-lookup"><span data-stu-id="63ed9-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  <span data-ttu-id="63ed9-135">Análises de produtos</span><span class="sxs-lookup"><span data-stu-id="63ed9-135">Product Reviews</span></span>

<span data-ttu-id="63ed9-136">Primeiro, adicionaremos um botão com um link para um formulário que podemos usar para inserir uma revisão do produto.</span><span class="sxs-lookup"><span data-stu-id="63ed9-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="63ed9-137">Observe que estamos passando o ProductID na cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="63ed9-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="63ed9-138">Próxima vamos adicionar a página chamada ReviewAdd.aspx</span><span class="sxs-lookup"><span data-stu-id="63ed9-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="63ed9-139">Esta página será usar o ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="63ed9-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="63ed9-140">Se você ainda não tiver feito para que você pode baixá-lo partir [DevExpress](http://devexpress.com/act) e há diretrizes sobre como configurar o Kit de ferramentas para uso com o Visual Studio aqui [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="63ed9-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="63ed9-141">No modo de design, arraste os controles e validadores da caixa de ferramentas e compilar um formulário como a mostrada abaixo.</span><span class="sxs-lookup"><span data-stu-id="63ed9-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="63ed9-142">A marcação será algo parecido com isso.</span><span class="sxs-lookup"><span data-stu-id="63ed9-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="63ed9-143">Agora que podemos inserir análises, vamos exibir essas análises na página do produto.</span><span class="sxs-lookup"><span data-stu-id="63ed9-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="63ed9-144">Adicione esta marcação à página ProductDetails.aspx.</span><span class="sxs-lookup"><span data-stu-id="63ed9-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="63ed9-145">Executando agora nosso aplicativo e navegando até um produto mostram as informações de produto, incluindo as revisões do cliente.</span><span class="sxs-lookup"><span data-stu-id="63ed9-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  <span data-ttu-id="63ed9-146">Controle de itens populares (Criando controles de usuário)</span><span class="sxs-lookup"><span data-stu-id="63ed9-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="63ed9-147">Para aumentar as vendas em seu site da web, adicionaremos alguns recursos para produtos populares ou relacionados de "origem de venda sugestivo".</span><span class="sxs-lookup"><span data-stu-id="63ed9-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="63ed9-148">O primeiro desses recursos será uma lista do produto no nosso catálogo de produtos mais popular.</span><span class="sxs-lookup"><span data-stu-id="63ed9-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="63ed9-149">Criaremos um "controle de usuário" para exibir os itens na home page do nosso aplicativo mais vendidos.</span><span class="sxs-lookup"><span data-stu-id="63ed9-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="63ed9-150">Uma vez que esse será um controle, podemos usar isso em qualquer página simplesmente arrastando e soltando o controle no designer do Visual Studio para qualquer página que gostamos de.</span><span class="sxs-lookup"><span data-stu-id="63ed9-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="63ed9-151">No Gerenciador de soluções do Visual Studio, clique com botão direito no nome da solução e crie um novo diretório chamado "Controles".</span><span class="sxs-lookup"><span data-stu-id="63ed9-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="63ed9-152">Embora não seja necessário fazer isso, podemos ajudará a manter nosso projeto, organizado por criar todos os nossos controles de usuário no diretório "Controles".</span><span class="sxs-lookup"><span data-stu-id="63ed9-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="63ed9-153">Clique com botão direito na pasta de controles e escolha "Novo Item":</span><span class="sxs-lookup"><span data-stu-id="63ed9-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="63ed9-154">Especifique um nome para o nosso controle de "PopularItems".</span><span class="sxs-lookup"><span data-stu-id="63ed9-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="63ed9-155">Observe que a extensão de arquivo para controles de usuário ASCX não. aspx.</span><span class="sxs-lookup"><span data-stu-id="63ed9-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="63ed9-156">Nosso controle de usuário populares de itens será definido da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="63ed9-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="63ed9-157">Aqui, estamos usando um método que não usamos ainda neste aplicativo.</span><span class="sxs-lookup"><span data-stu-id="63ed9-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="63ed9-158">Estamos usando o controle repeater e, em vez de usar um controle de fonte de dados estamos vinculando o controle Repeater aos resultados de uma consulta LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="63ed9-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="63ed9-159">No code-behind do nosso controle fazemos isso da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="63ed9-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="63ed9-160">Observe também esta linha importante na parte superior da marcação do nosso controle.</span><span class="sxs-lookup"><span data-stu-id="63ed9-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="63ed9-161">Uma vez que os itens mais populares não será alterada em uma base de minuto a minuto, podemos adicionar uma diretiva dor para melhorar o desempenho do nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="63ed9-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="63ed9-162">Essa diretiva fará com que o código de controles para ser executado apenas quando a saída do controle do cache expira.</span><span class="sxs-lookup"><span data-stu-id="63ed9-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="63ed9-163">Caso contrário, a versão em cache de saída do controle será usada.</span><span class="sxs-lookup"><span data-stu-id="63ed9-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="63ed9-164">Agora tudo o que precisamos fazer é incluir nosso novo controle na nossa página Default.aspc.</span><span class="sxs-lookup"><span data-stu-id="63ed9-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="63ed9-165">Use arrastar e soltar para colocar uma instância do controle na coluna de nosso formulário de padrão aberta.</span><span class="sxs-lookup"><span data-stu-id="63ed9-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="63ed9-166">Agora, quando executamos nosso aplicativo a home page exibe os itens mais populares.</span><span class="sxs-lookup"><span data-stu-id="63ed9-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  <span data-ttu-id="63ed9-167">"Também adquiridos" controlam (controles de usuário com parâmetros)</span><span class="sxs-lookup"><span data-stu-id="63ed9-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="63ed9-168">O segundo controle de usuário que vamos criar levará sugestivo de venda para o próximo nível com a adição de especificidade do contexto.</span><span class="sxs-lookup"><span data-stu-id="63ed9-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="63ed9-169">A lógica para calcular os principais itens "Também adquiridos" é não trivial.</span><span class="sxs-lookup"><span data-stu-id="63ed9-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="63ed9-170">Nosso controle "Também adquiridos" será selecionar os registros de OrderDetails (comprados anteriormente) para o ProductID selecionado no momento e pegue o OrderIDs para cada pedido exclusivo que é encontrado.</span><span class="sxs-lookup"><span data-stu-id="63ed9-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="63ed9-171">Em seguida, selecionaremos al os produtos de todos esses pedidos e soma as quantidades compradas.</span><span class="sxs-lookup"><span data-stu-id="63ed9-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="63ed9-172">Vamos classificar os produtos que totalizam de quantidade e exibir os cinco principais itens.</span><span class="sxs-lookup"><span data-stu-id="63ed9-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="63ed9-173">Dada a complexidade da lógica do programa, podemos irá implementar esse algoritmo como um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="63ed9-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="63ed9-174">O T-SQL para o procedimento armazenado é da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="63ed9-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="63ed9-175">Observe que esse procedimento armazenado (SelectPurchasedWithProducts) existia no banco de dados quando é incluído em nosso aplicativo e quando geramos o modelo de dados de entidade que especificamos que, além de tabelas e exibições que precisávamos fazer isso, o modelo de dados de entidade deve incluir esse procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="63ed9-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="63ed9-176">Para acessar o procedimento armazenado do modelo de dados de entidade, é preciso importar a função.</span><span class="sxs-lookup"><span data-stu-id="63ed9-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="63ed9-177">Clique duas vezes no modelo de dados de entidade no Gerenciador de soluções para abri-lo no designer e abra o navegador de modelo, e em seguida, clique com botão direito no designer e selecione "Adicionar importação de função".</span><span class="sxs-lookup"><span data-stu-id="63ed9-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="63ed9-178">Ao fazer isso, essa caixa de diálogo será aberta.</span><span class="sxs-lookup"><span data-stu-id="63ed9-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="63ed9-179">Preencha os campos, como você pode ver acima, selecionando "SelectPurchasedWithProducts" e use o nome do procedimento para o nome da nossa função importada.</span><span class="sxs-lookup"><span data-stu-id="63ed9-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="63ed9-180">Clique em "Okey".</span><span class="sxs-lookup"><span data-stu-id="63ed9-180">Click "Ok".</span></span>

<span data-ttu-id="63ed9-181">Tendo feito isso, que podemos simplesmente pode programar o procedimento armazenado como nós podem qualquer outro item no modelo.</span><span class="sxs-lookup"><span data-stu-id="63ed9-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="63ed9-182">Portanto, em nossa pasta de "Controles" Crie um novo controle de usuário chamado AlsoPurchased.ascx.</span><span class="sxs-lookup"><span data-stu-id="63ed9-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="63ed9-183">A marcação para esse controle parecerá muito familiar para o controle PopularItems.</span><span class="sxs-lookup"><span data-stu-id="63ed9-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="63ed9-184">A diferença notável é que não armazenados em cache a saída, pois o item a ser renderizado variam por produto.</span><span class="sxs-lookup"><span data-stu-id="63ed9-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="63ed9-185">O ProductId será "property" para o controle.</span><span class="sxs-lookup"><span data-stu-id="63ed9-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="63ed9-186">No manipulador do evento PreRender do controle é eed fazer três coisas.</span><span class="sxs-lookup"><span data-stu-id="63ed9-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="63ed9-187">Verifique se que o ProductID está definido.</span><span class="sxs-lookup"><span data-stu-id="63ed9-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="63ed9-188">Consulte se houver produtos que foram adquiridos com a atual.</span><span class="sxs-lookup"><span data-stu-id="63ed9-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="63ed9-189">Alguns itens, conforme determinado na #2 de saída.</span><span class="sxs-lookup"><span data-stu-id="63ed9-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="63ed9-190">Observe como é fácil chamar o procedimento armazenado por meio do modelo.</span><span class="sxs-lookup"><span data-stu-id="63ed9-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="63ed9-191">Depois de determinar que há são "também adquiridos" podemos simplesmente fazer a ligação repetidor aos resultados retornados pela consulta.</span><span class="sxs-lookup"><span data-stu-id="63ed9-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="63ed9-192">Se não houvesse todos os itens "também adquiridos" Vamos simplesmente exibir outros itens populares do nosso catálogo.</span><span class="sxs-lookup"><span data-stu-id="63ed9-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="63ed9-193">Para exibir os itens "Também comprou", abra a página ProductDetails.aspx e arraste o controle de AlsoPurchased do Gerenciador de soluções, para que ele apareça nessa posição na marcação.</span><span class="sxs-lookup"><span data-stu-id="63ed9-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="63ed9-194">Ao fazer isso, você criará uma referência ao controle na parte superior da página ProductDetails.</span><span class="sxs-lookup"><span data-stu-id="63ed9-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="63ed9-195">Uma vez que o controle de usuário AlsoPurchased requer um número de ProductId, definimos a propriedade ProductID do nosso controle por meio de uma instrução Eval no item de modelo de dados atual da página.</span><span class="sxs-lookup"><span data-stu-id="63ed9-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="63ed9-196">Quando criamos e executar agora e navegue até um produto, podemos ver os itens "Também adquiridos".</span><span class="sxs-lookup"><span data-stu-id="63ed9-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="63ed9-197">[Anterior](tailspin-spyworks-part-6.md)
> [Próximo](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="63ed9-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>

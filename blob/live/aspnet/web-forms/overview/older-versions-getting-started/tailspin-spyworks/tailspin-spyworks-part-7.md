---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Parte 7: Adicionando recursos | Microsoft Docs'
author: JoeStagner
description: "Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 7 adiciona recursos adicionais, como a conta revie..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 5280de44b3e75f9d1ae85e0248bc3ef6d5444f6d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="part-7-adding-features"></a><span data-ttu-id="917f5-104">Parte 7: Adicionar recursos</span><span class="sxs-lookup"><span data-stu-id="917f5-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="917f5-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="917f5-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="917f5-106">Tailspin Spyworks demonstra como extraordinariamente simples é criar aplicativos eficientes e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="917f5-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="917f5-107">Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, inclusive de compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="917f5-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="917f5-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="917f5-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="917f5-109">Parte 7 adiciona recursos adicionais, como análise de conta, análises de produtos e "itens populares" e controles de usuário "também comprado".</span><span class="sxs-lookup"><span data-stu-id="917f5-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a><span data-ttu-id="917f5-110">Adicionando recursos</span><span class="sxs-lookup"><span data-stu-id="917f5-110">Adding Features</span></span>

<span data-ttu-id="917f5-111">Embora os usuários podem procurar nosso catálogo, colocar itens no carrinho de compras e concluir o processo de check-out, há que um número de dar suporte a recursos que serão incluídos para melhorar o nosso site.</span><span class="sxs-lookup"><span data-stu-id="917f5-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="917f5-112">Revisão de conta (lista de pedidos colocados e exibir mais detalhes.)</span><span class="sxs-lookup"><span data-stu-id="917f5-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="917f5-113">Acrescente conteúdo específico de contexto para a página inicial.</span><span class="sxs-lookup"><span data-stu-id="917f5-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="917f5-114">Adicione um recurso para permitir que usuários revisar os produtos no catálogo.</span><span class="sxs-lookup"><span data-stu-id="917f5-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="917f5-115">Crie um controle de usuário para exibir itens populares e local que controlam na página inicial.</span><span class="sxs-lookup"><span data-stu-id="917f5-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="917f5-116">Criar um controle de usuário "Também adquirido" e adicione-o para a página de detalhes do produto.</span><span class="sxs-lookup"><span data-stu-id="917f5-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="917f5-117">Adicionar um contato de página.</span><span class="sxs-lookup"><span data-stu-id="917f5-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="917f5-118">Adicionar um sobre a página.</span><span class="sxs-lookup"><span data-stu-id="917f5-118">Add an About Page.</span></span>
8. <span data-ttu-id="917f5-119">Erro global</span><span class="sxs-lookup"><span data-stu-id="917f5-119">Global Error</span></span>

## <a id="_Toc260221674"></a><span data-ttu-id="917f5-120">Revisão de conta</span><span class="sxs-lookup"><span data-stu-id="917f5-120">Account Review</span></span>

<span data-ttu-id="917f5-121">Na pasta "Conta", crie duas páginas. aspx um OrderList.aspx nomeado e a outros OrderDetails.aspx nomeado</span><span class="sxs-lookup"><span data-stu-id="917f5-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="917f5-122">OrderList.aspx aproveitará os controles GridView e EntityDataSoure como temos anteriormente.</span><span class="sxs-lookup"><span data-stu-id="917f5-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="917f5-123">O EntityDataSoure seleciona os registros da tabela filtrada no nome de usuário (consulte o WhereParameter) que é definido em uma variável de sessão quando o logon do usuário 's.</span><span class="sxs-lookup"><span data-stu-id="917f5-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="917f5-124">Observe também esses parâmetros no HyperlinkField de GridView:</span><span class="sxs-lookup"><span data-stu-id="917f5-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="917f5-125">Elas especificam o link para a exibição de detalhes do pedido para cada produto especificando o campo OrderID como um parâmetro QueryString para a página OrderDetails.aspx.</span><span class="sxs-lookup"><span data-stu-id="917f5-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a><span data-ttu-id="917f5-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="917f5-126">OrderDetails.aspx</span></span>

<span data-ttu-id="917f5-127">Usaremos um controle EntityDataSource para acessar os pedidos e um FormView para exibir os dados de pedidos e outro EntityDataSource com um controle GridView para exibir os itens de linha de todos os da ordem.</span><span class="sxs-lookup"><span data-stu-id="917f5-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="917f5-128">O código por trás de arquivo (OrderDetails.aspx.cs) temos dois bits pouco de manutenção do sistema.</span><span class="sxs-lookup"><span data-stu-id="917f5-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="917f5-129">Primeiro, precisamos garantir que OrderDetails sempre obtém um OrderId.</span><span class="sxs-lookup"><span data-stu-id="917f5-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="917f5-130">Também precisamos calcular e exibir o total de itens de linha do pedido.</span><span class="sxs-lookup"><span data-stu-id="917f5-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a><span data-ttu-id="917f5-131">A Home Page</span><span class="sxs-lookup"><span data-stu-id="917f5-131">The Home Page</span></span>

<span data-ttu-id="917f5-132">Vamos adicionar alguns conteúdo estático para a página Default.aspx.</span><span class="sxs-lookup"><span data-stu-id="917f5-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="917f5-133">Primeiro, vou criar uma pasta "Conteúda" e dentro de uma pasta de imagens (e incluirá uma imagem a ser usada na página inicial).</span><span class="sxs-lookup"><span data-stu-id="917f5-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="917f5-134">Para o espaço reservado de parte inferior da página Default.aspx, adicione a seguinte marcação.</span><span class="sxs-lookup"><span data-stu-id="917f5-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a><span data-ttu-id="917f5-135">Análises de produtos</span><span class="sxs-lookup"><span data-stu-id="917f5-135">Product Reviews</span></span>

<span data-ttu-id="917f5-136">Primeiro, adicionaremos um botão com um link para um formulário que podemos usar para inserir uma revisão do produto.</span><span class="sxs-lookup"><span data-stu-id="917f5-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="917f5-137">Observe que, passa o ProductID na cadeia de caracteres de consulta</span><span class="sxs-lookup"><span data-stu-id="917f5-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="917f5-138">Avançar vamos adicionar página chamada ReviewAdd.aspx</span><span class="sxs-lookup"><span data-stu-id="917f5-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="917f5-139">Essa página usará o Kit de ferramentas de controle do ASP.NET AJAX.</span><span class="sxs-lookup"><span data-stu-id="917f5-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="917f5-140">Se você ainda não tiver feito para você pode baixá-lo do [DevExpress](http://devexpress.com/act) e há diretrizes sobre como configurar o Kit de ferramentas para uso com o Visual Studio aqui [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="917f5-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="917f5-141">No modo design, arraste validadores e controles da caixa de ferramentas e criar um formulário como a mostrada abaixo.</span><span class="sxs-lookup"><span data-stu-id="917f5-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="917f5-142">A marcação será parecida com isto.</span><span class="sxs-lookup"><span data-stu-id="917f5-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="917f5-143">Agora que podemos inserir revisões, permite exibir as revisões na página do produto.</span><span class="sxs-lookup"><span data-stu-id="917f5-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="917f5-144">Adicione esta marcação para a página ProductDetails.aspx.</span><span class="sxs-lookup"><span data-stu-id="917f5-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="917f5-145">Executando o nosso aplicativo agora e navegar para um produto mostram as informações de produto, incluindo revisões de cliente.</span><span class="sxs-lookup"><span data-stu-id="917f5-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a><span data-ttu-id="917f5-146">Controle de itens populares (criação de controles de usuário)</span><span class="sxs-lookup"><span data-stu-id="917f5-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="917f5-147">Para aumentar as vendas em seu site da web vamos adicionar alguns recursos para produtos de "venda que sugere" populares ou relacionados.</span><span class="sxs-lookup"><span data-stu-id="917f5-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="917f5-148">O primeiro desses recursos será uma lista do produto mais popular do nosso catálogo de produtos.</span><span class="sxs-lookup"><span data-stu-id="917f5-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="917f5-149">Vamos criar um "controle de usuário" para exibir o itens na página inicial do nosso aplicativo vendido.</span><span class="sxs-lookup"><span data-stu-id="917f5-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="917f5-150">Já que isso será um controle, podemos usar isso em qualquer página simplesmente arrastando e soltando o controle no designer do Visual Studio para qualquer página que gostamos.</span><span class="sxs-lookup"><span data-stu-id="917f5-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="917f5-151">No Gerenciador de soluções do Visual Studio, clique com botão direito no nome da solução e crie um novo diretório chamado "Controles".</span><span class="sxs-lookup"><span data-stu-id="917f5-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="917f5-152">Embora não seja necessário fazer isso, nós o ajudaremos manter nosso projeto criando todos os controles de usuário no diretório "Controles".</span><span class="sxs-lookup"><span data-stu-id="917f5-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="917f5-153">Com o botão direito na pasta de controles e escolha "New Item":</span><span class="sxs-lookup"><span data-stu-id="917f5-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="917f5-154">Especifique um nome para o nosso controle de "PopularItems".</span><span class="sxs-lookup"><span data-stu-id="917f5-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="917f5-155">Observe que a extensão de arquivo para controles de usuário é. ascx não. aspx.</span><span class="sxs-lookup"><span data-stu-id="917f5-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="917f5-156">Nosso controle de usuário populares de itens será definido da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="917f5-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="917f5-157">Aqui, estamos usando um método que não usamos ainda neste aplicativo.</span><span class="sxs-lookup"><span data-stu-id="917f5-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="917f5-158">Estamos usando o controle repetidor e em vez de usar um controle de fonte de dados estamos vinculando controle repetidor para os resultados de uma consulta LINQ to Entities.</span><span class="sxs-lookup"><span data-stu-id="917f5-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="917f5-159">No code-behind de nosso controle podemos fazer isso da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="917f5-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="917f5-160">Observe também esta linha importante na parte superior da marcação do nosso controle.</span><span class="sxs-lookup"><span data-stu-id="917f5-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="917f5-161">Como os itens mais populares não será alterada em uma base de minuto a minuto podemos adicionar uma diretiva dor para melhorar o desempenho do nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="917f5-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="917f5-162">Essa diretiva fará com que o código de controles ser executado apenas quando a saída do controle do cache expira.</span><span class="sxs-lookup"><span data-stu-id="917f5-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="917f5-163">Caso contrário, a versão armazenada em cache de saída do controle será usada.</span><span class="sxs-lookup"><span data-stu-id="917f5-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="917f5-164">Agora tudo o que precisamos fazer é inclua nosso novo controle em nossa página de Default.aspc.</span><span class="sxs-lookup"><span data-stu-id="917f5-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="917f5-165">Use arrastar e soltar para colocar uma instância do controle na coluna de nosso formulário de padrão aberta.</span><span class="sxs-lookup"><span data-stu-id="917f5-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="917f5-166">Agora, quando executamos nosso aplicativo home page exibe os itens mais populares.</span><span class="sxs-lookup"><span data-stu-id="917f5-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a><span data-ttu-id="917f5-167">"Também adquirido" controla (controles de usuário com parâmetros)</span><span class="sxs-lookup"><span data-stu-id="917f5-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="917f5-168">O segundo controle de usuário que vamos criar terão que sugere de vendas para o próximo nível com a adição de especificidade de contexto.</span><span class="sxs-lookup"><span data-stu-id="917f5-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="917f5-169">A lógica para calcular os principais itens "Também adquirido" é incomum.</span><span class="sxs-lookup"><span data-stu-id="917f5-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="917f5-170">Nosso controle "Também adquirido" selecionar os registros de OrderDetails (tenha comprados) para o ProductID selecionado no momento e obter OrderIDs para cada pedido exclusivo que é encontrado.</span><span class="sxs-lookup"><span data-stu-id="917f5-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="917f5-171">Em seguida, selecionaremos todos os produtos de todos esses pedidos e soma as quantidades compradas.</span><span class="sxs-lookup"><span data-stu-id="917f5-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="917f5-172">Vamos classificar os produtos que totalizam quantidade e exibir os cinco principais itens.</span><span class="sxs-lookup"><span data-stu-id="917f5-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="917f5-173">Dada a complexidade da lógica do programa, vamos implementar esse algoritmo como um procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="917f5-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="917f5-174">O T-SQL para o procedimento armazenado é o seguinte.</span><span class="sxs-lookup"><span data-stu-id="917f5-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="917f5-175">Observe que esse procedimento armazenado (SelectPurchasedWithProducts) existe no banco de dados quando é incluído em nosso aplicativo e é gerado o modelo de dados de entidade especificamos que, além das tabelas e exibições que são necessários, o modelo de dados de entidade deve incluir esse procedimento armazenado.</span><span class="sxs-lookup"><span data-stu-id="917f5-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="917f5-176">Para acessar o procedimento armazenado do modelo de dados de entidade, é preciso importar a função.</span><span class="sxs-lookup"><span data-stu-id="917f5-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="917f5-177">Clique duas vezes no modelo de dados de entidade no Gerenciador de soluções para abri-lo no designer e abrir o navegador de modelo, em seguida, com o botão direito no designer e selecione "Adicionar importação de função".</span><span class="sxs-lookup"><span data-stu-id="917f5-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="917f5-178">Ao fazer isso, esta caixa de diálogo será aberta.</span><span class="sxs-lookup"><span data-stu-id="917f5-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="917f5-179">Preencha os campos que você vê acima, selecionando "SelectPurchasedWithProducts" e use o nome do procedimento para o nome da nossa função importada.</span><span class="sxs-lookup"><span data-stu-id="917f5-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="917f5-180">Clique em "Okey".</span><span class="sxs-lookup"><span data-stu-id="917f5-180">Click "Ok".</span></span>

<span data-ttu-id="917f5-181">Ter feito isso, simplesmente pode programar o procedimento armazenado é como qualquer outro item no modelo.</span><span class="sxs-lookup"><span data-stu-id="917f5-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="917f5-182">Portanto, em nossa pasta de "Controles" Crie um novo controle de usuário chamado AlsoPurchased.ascx.</span><span class="sxs-lookup"><span data-stu-id="917f5-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="917f5-183">A marcação para este controle parecerá muito familiar para o controle PopularItems.</span><span class="sxs-lookup"><span data-stu-id="917f5-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="917f5-184">A diferença notável é que não armazenados em cache de saída desde que o item a ser renderizado variam por produto.</span><span class="sxs-lookup"><span data-stu-id="917f5-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="917f5-185">O ProductId será "property" para o controle.</span><span class="sxs-lookup"><span data-stu-id="917f5-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="917f5-186">No manipulador de evento PreRender do controle é sangramento três itens.</span><span class="sxs-lookup"><span data-stu-id="917f5-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="917f5-187">Verifique se que o ProductID está definido.</span><span class="sxs-lookup"><span data-stu-id="917f5-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="917f5-188">Consulte se há quaisquer produtos foram comprados com a atual.</span><span class="sxs-lookup"><span data-stu-id="917f5-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="917f5-189">Saída de alguns itens conforme determinado na #2.</span><span class="sxs-lookup"><span data-stu-id="917f5-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="917f5-190">Observe como é fácil chamar o procedimento armazenado por meio do modelo.</span><span class="sxs-lookup"><span data-stu-id="917f5-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="917f5-191">Depois de determinar o que há "também comprados" podemos simplesmente associar repetidor aos resultados retornados pela consulta.</span><span class="sxs-lookup"><span data-stu-id="917f5-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="917f5-192">Se não havia nenhum item "também adquirido" simplesmente exibiremos outros itens populares do nosso catálogo.</span><span class="sxs-lookup"><span data-stu-id="917f5-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="917f5-193">Para exibir os itens "Também adquirido", abra a página ProductDetails.aspx e arraste o controle de AlsoPurchased do Gerenciador de soluções para que ele apareça nessa posição na marcação.</span><span class="sxs-lookup"><span data-stu-id="917f5-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="917f5-194">Isso criará uma referência para o controle na parte superior da página ProductDetails.</span><span class="sxs-lookup"><span data-stu-id="917f5-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="917f5-195">Como o controle de usuário AlsoPurchased requer um número de ProductId, vamos definir a propriedade ProductID de nosso controle usando uma instrução Eval em relação ao item de modelo de dados atual da página.</span><span class="sxs-lookup"><span data-stu-id="917f5-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="917f5-196">Quando podemos criar e executar agora e navegue até um produto, podemos ver os itens "Também adquirido".</span><span class="sxs-lookup"><span data-stu-id="917f5-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

>[!div class="step-by-step"]
<span data-ttu-id="917f5-197">[Anterior](tailspin-spyworks-part-6.md)
[Próximo](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="917f5-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>

---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: Lista os produtos | Microsoft Docs'
author: JoeStagner
description: "Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 4 abrange produtos de listagem com contr. o GridView."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 420cdbcc002bcbfff619d399a7a374009999d754
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="part-4-listing-products"></a><span data-ttu-id="10fc1-104">Parte 4: Lista de produtos</span><span class="sxs-lookup"><span data-stu-id="10fc1-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="10fc1-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="10fc1-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="10fc1-106">Tailspin Spyworks demonstra como extraordinariamente simples é criar aplicativos eficientes e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="10fc1-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="10fc1-107">Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, inclusive de compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="10fc1-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="10fc1-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="10fc1-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="10fc1-109">Parte 4 abrange a listagem de produtos com o controle GridView.</span><span class="sxs-lookup"><span data-stu-id="10fc1-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a><span data-ttu-id="10fc1-110">Lista os produtos com o controle GridView</span><span class="sxs-lookup"><span data-stu-id="10fc1-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="10fc1-111">Vamos começar a implementar nossa página ProductsList.aspx clicando em"direita" em nossa solução e selecionando "Adicionar" e "Novo Item".</span><span class="sxs-lookup"><span data-stu-id="10fc1-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="10fc1-112">Escolha "Formulário usando página mestra dos Web" e insira um nome de página de ProductsList.aspx".</span><span class="sxs-lookup"><span data-stu-id="10fc1-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="10fc1-113">Clique em "Adicionar".</span><span class="sxs-lookup"><span data-stu-id="10fc1-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="10fc1-114">Em seguida, escolha a pasta "Estilos" onde colocamos a página Site.Master e selecioná-lo na janela "Conteúdo da pasta".</span><span class="sxs-lookup"><span data-stu-id="10fc1-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="10fc1-115">Clique em "Okey" para criar a página.</span><span class="sxs-lookup"><span data-stu-id="10fc1-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="10fc1-116">Nosso banco de dados é preenchido com dados de produto, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="10fc1-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="10fc1-117">Após a criação de nossa página novamente usaremos uma fonte de dados de entidade para acessar esses dados de produto, mas nesta instância, precisamos selecionar as entidades Product e é necessário restringir os itens que são retornados a apenas aqueles para a categoria selecionada.</span><span class="sxs-lookup"><span data-stu-id="10fc1-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="10fc1-118">Para fazer isso informaremos EntityDataSource para gerar automaticamente a cláusula WHERE e especificaremos a WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="10fc1-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="10fc1-119">Você deve se lembrar que quando criamos os itens de Menu em nosso "Menu de categoria de produto" dinamicamente criamos o link, adicionando o CatagoryID para QueryString para cada link.</span><span class="sxs-lookup"><span data-stu-id="10fc1-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="10fc1-120">A fonte de dados de entidade derivar o parâmetro onde esse parâmetro QueryString será informado.</span><span class="sxs-lookup"><span data-stu-id="10fc1-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="10fc1-121">Em seguida, configuraremos o controle ListView para exibir uma lista de produtos.</span><span class="sxs-lookup"><span data-stu-id="10fc1-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="10fc1-122">Para criar uma experiência ideal de compra que vai compactar vários recursos concisos para cada produto individual exibido em nosso ListVew.</span><span class="sxs-lookup"><span data-stu-id="10fc1-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="10fc1-123">O nome do produto será um link para a exibição de detalhes do produto.</span><span class="sxs-lookup"><span data-stu-id="10fc1-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="10fc1-124">O preço do produto será exibido.</span><span class="sxs-lookup"><span data-stu-id="10fc1-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="10fc1-125">Uma imagem do produto será exibida e dinamicamente selecionaremos a imagem de um diretório de imagens de catálogo em nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="10fc1-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="10fc1-126">Podemos incluem um link para adicionar o produto específico imediatamente ao carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="10fc1-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="10fc1-127">Aqui está a marcação para a instância do controle ListView.</span><span class="sxs-lookup"><span data-stu-id="10fc1-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="10fc1-128">Estamos criando vários links para cada produto exibido dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="10fc1-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="10fc1-129">Além disso, antes de testamos própria nova página precisamos criar a estrutura de diretórios para o produto imagens de catálogo da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="10fc1-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="10fc1-130">Depois que a nossas imagens de produto estão acessíveis podemos testar nossa página de lista do produto.</span><span class="sxs-lookup"><span data-stu-id="10fc1-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="10fc1-131">Na home page do site, clique em um dos Links de lista de categoria.</span><span class="sxs-lookup"><span data-stu-id="10fc1-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="10fc1-132">Agora, precisamos implementar a página ProductDetials.apsx e a funcionalidade de AddToCart.</span><span class="sxs-lookup"><span data-stu-id="10fc1-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="10fc1-133">Use o arquivo -&gt;novo para criar um nome de página ProductDetails.aspx usando o site de página mestra como fizemos anteriormente.</span><span class="sxs-lookup"><span data-stu-id="10fc1-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="10fc1-134">Nós usaremos novamente um controle EntityDataSource para acessar o registro de produto específico no banco de dados e usaremos um controle FormView do ASP.NET para exibir os dados de produto da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="10fc1-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="10fc1-135">Não se preocupe se a formatação parecer um pouco estranho para você.</span><span class="sxs-lookup"><span data-stu-id="10fc1-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="10fc1-136">A marcação acima deixa espaço no layout de exibição para alguns recursos que implementaremos posteriormente.</span><span class="sxs-lookup"><span data-stu-id="10fc1-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="10fc1-137">Representa uma lógica mais complexa no nosso aplicativo de carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="10fc1-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="10fc1-138">Para começar, use o arquivo -&gt;novo para criar uma página chamada MyShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="10fc1-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="10fc1-139">Observe que não estamos optando o nome ShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="10fc1-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="10fc1-140">Nosso banco de dados contém uma tabela chamada "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="10fc1-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="10fc1-141">Quando é gerado um modelo de dados de entidade foi criada uma classe para cada tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="10fc1-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="10fc1-142">Portanto, o modelo de dados de entidade gerado de uma classe de entidade nomeada "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="10fc1-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="10fc1-143">Podemos pode editar o modelo para que podemos pode usar esse nome para a implementação de carrinho de compras ou estendê-lo para nossas necessidades, mas é aceita em vez disso, para selecionar apenas um nome que pode evitar o conflito.</span><span class="sxs-lookup"><span data-stu-id="10fc1-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="10fc1-144">Também vale a pena observar que estamos criando um carrinho de compras simple e inserindo a lógica de carrinho de compras com a exibição do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="10fc1-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="10fc1-145">Podemos também pode optar por implementar nosso carrinho de compras em uma camada de negócios totalmente separada.</span><span class="sxs-lookup"><span data-stu-id="10fc1-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="10fc1-146">[Anterior](tailspin-spyworks-part-3.md)
[Próximo](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="10fc1-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>

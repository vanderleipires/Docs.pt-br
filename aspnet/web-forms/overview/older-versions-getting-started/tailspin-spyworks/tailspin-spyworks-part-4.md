---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: Lista os produtos | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 4 aborda a listagem de produtos com o GridView contr....
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: ec349a2564a63fd950ca5f6a171e6ffd1199c28a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813066"
---
<a name="part-4-listing-products"></a><span data-ttu-id="3216e-104">Parte 4: Listagem de produtos</span><span class="sxs-lookup"><span data-stu-id="3216e-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="3216e-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="3216e-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="3216e-106">Tailspin Spyworks demonstra como incrivelmente simples é criar aplicativos avançados e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="3216e-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="3216e-107">Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, incluindo as compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="3216e-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="3216e-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="3216e-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="3216e-109">Parte 4 aborda a listagem de produtos com o controle GridView.</span><span class="sxs-lookup"><span data-stu-id="3216e-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a>  <span data-ttu-id="3216e-110">Listagem de produtos com o controle GridView</span><span class="sxs-lookup"><span data-stu-id="3216e-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="3216e-111">Vamos começar a implementar nossa página ProductsList.aspx clicando em"com o botão direito" em nossa solução e selecionando "Adicionar" e "Novo Item".</span><span class="sxs-lookup"><span data-stu-id="3216e-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="3216e-112">Escolha "Formulário usando página mestra dos Web" e insira um nome de página de ProductsList.aspx".</span><span class="sxs-lookup"><span data-stu-id="3216e-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="3216e-113">Clique em "Adicionar".</span><span class="sxs-lookup"><span data-stu-id="3216e-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="3216e-114">Em seguida, escolha a pasta "Estilos" onde colocamos a página Master e selecioná-lo na janela de "Conteúdo da pasta".</span><span class="sxs-lookup"><span data-stu-id="3216e-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="3216e-115">Clique em "Okey" para criar a página.</span><span class="sxs-lookup"><span data-stu-id="3216e-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="3216e-116">Nosso banco de dados é preenchido com dados do produto, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="3216e-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="3216e-117">Depois que nossa página é criada, usaremos novamente uma fonte de dados de entidade para acessar esses dados de produto, mas nesta instância, precisamos selecionar as entidades Product e precisamos restringir os itens que são retornados a apenas aqueles para a categoria selecionada.</span><span class="sxs-lookup"><span data-stu-id="3216e-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="3216e-118">Para fazer isso, informaremos o EntityDataSource para gerar automaticamente a cláusula WHERE e especificaremos o WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="3216e-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="3216e-119">Você irá se lembrar que quando criamos os itens de Menu no nosso "Menu de categoria de produto" é criada dinamicamente o link, adicionando o CatagoryID a cadeia de consulta para cada link.</span><span class="sxs-lookup"><span data-stu-id="3216e-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="3216e-120">A fonte de dados de entidade para derivar o parâmetro de onde esse parâmetro QueryString será informado.</span><span class="sxs-lookup"><span data-stu-id="3216e-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="3216e-121">Em seguida, configuraremos o controle ListView para exibir uma lista de produtos.</span><span class="sxs-lookup"><span data-stu-id="3216e-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="3216e-122">Para criar uma experiência ideal de compra que podemos irá compactar vários recursos concisos para cada produto individual exibido em nossa ListVew.</span><span class="sxs-lookup"><span data-stu-id="3216e-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="3216e-123">O nome do produto será um link para a exibição de detalhes do produto.</span><span class="sxs-lookup"><span data-stu-id="3216e-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="3216e-124">O preço do produto será exibido.</span><span class="sxs-lookup"><span data-stu-id="3216e-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="3216e-125">Uma imagem do produto será exibida e dinamicamente selecionaremos a imagem de um diretório de imagens de catálogo em nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3216e-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="3216e-126">Incluiremos um link para adicionar imediatamente o produto específico ao carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="3216e-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="3216e-127">Aqui está a marcação para a nossa instância do controle ListView.</span><span class="sxs-lookup"><span data-stu-id="3216e-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="3216e-128">Estamos criando vários links para cada produto exibido dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="3216e-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="3216e-129">Além disso, antes que podemos testar a nova página do própria precisamos criar a estrutura de diretórios para o produto imagens do catálogo da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="3216e-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="3216e-130">Depois que as nossas imagens de produto são acessíveis podemos testar nossa página de lista do produto.</span><span class="sxs-lookup"><span data-stu-id="3216e-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="3216e-131">Na home page do site, clique em um dos Links da lista de categoria.</span><span class="sxs-lookup"><span data-stu-id="3216e-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="3216e-132">Agora, precisamos implementar a página ProductDetials.apsx e a funcionalidade de AddToCart.</span><span class="sxs-lookup"><span data-stu-id="3216e-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="3216e-133">Use o arquivo -&gt;New para criar um nome de página ProductDetails.aspx usando o página mestra do site, como fizemos anteriormente.</span><span class="sxs-lookup"><span data-stu-id="3216e-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="3216e-134">Nós usaremos novamente um controle EntityDataSource para acessar o registro de produto específico no banco de dados e usaremos um controle ASP.NET FormView para exibir os dados de produto da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="3216e-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="3216e-135">Não se preocupe se a formatação se parece um pouco estranho para você.</span><span class="sxs-lookup"><span data-stu-id="3216e-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="3216e-136">A marcação acima deixa espaço no layout de exibição para alguns recursos, implementaremos posteriormente.</span><span class="sxs-lookup"><span data-stu-id="3216e-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="3216e-137">O carrinho de compras representará uma lógica mais complexa em nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3216e-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="3216e-138">Para começar, use o arquivo -&gt;novo para criar uma página chamada MyShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="3216e-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="3216e-139">Observe que não estamos escolhendo o nome ShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="3216e-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="3216e-140">Nosso banco de dados contém uma tabela chamada "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="3216e-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="3216e-141">Quando geramos um modelo de dados de entidade foi criada uma classe para cada tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3216e-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="3216e-142">Portanto, o modelo de dados de entidade gerado de uma classe de entidade chamada "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="3216e-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="3216e-143">Podemos pode editar o modelo para que podemos pode usar esse nome para nossa implementação de carrinho de compras ou estendê-lo para nossas necessidades, mas optamos por em vez disso, para selecionar apenas um nome que evitará o conflito.</span><span class="sxs-lookup"><span data-stu-id="3216e-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="3216e-144">Também vale a pena observar que estamos criando um carrinho de compras simple e incorporar a lógica de carrinho de compras com a exibição de carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="3216e-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="3216e-145">Podemos também pode optar por implementar nosso carrinho de compras em uma camada de negócios completamente separada.</span><span class="sxs-lookup"><span data-stu-id="3216e-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3216e-146">[Anterior](tailspin-spyworks-part-3.md)
> [Próximo](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="3216e-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>

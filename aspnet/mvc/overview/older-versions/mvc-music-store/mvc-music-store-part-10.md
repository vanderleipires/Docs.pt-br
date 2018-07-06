---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Parte 10: Atualizações Final para navegação e Design de Site, conclusão | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 10 aborda atualizações finais à navegação e S....
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: 40ed7c337e097675199ab66229095bd3315d8c8d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836800"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="46394-104">Parte 10: Atualizações Final para navegação e Design de Site, conclusão</span><span class="sxs-lookup"><span data-stu-id="46394-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="46394-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="46394-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="46394-106">A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.</span><span class="sxs-lookup"><span data-stu-id="46394-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="46394-107">A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="46394-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="46394-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="46394-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="46394-109">Parte 10 abrange atualizações finais à navegação e Design de Site, conclusão.</span><span class="sxs-lookup"><span data-stu-id="46394-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="46394-110">Concluímos toda a funcionalidade principal para o nosso site, mas ainda temos alguns recursos para adicionar a navegação do site, a home page e a página Procurar Store.</span><span class="sxs-lookup"><span data-stu-id="46394-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="46394-111">Criando a exibição parcial resumo de carrinho compras</span><span class="sxs-lookup"><span data-stu-id="46394-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="46394-112">Queremos expor o número de itens no carrinho de compras do usuário em todo o site.</span><span class="sxs-lookup"><span data-stu-id="46394-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="46394-113">Podemos facilmente implementar isso criando uma exibição parcial que é adicionada ao nosso site.</span><span class="sxs-lookup"><span data-stu-id="46394-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="46394-114">Conforme mostrado anteriormente, o controlador ShoppingCart inclui um método de ação de CartSummary que retorna uma exibição parcial:</span><span class="sxs-lookup"><span data-stu-id="46394-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="46394-115">Para criar a exibição parcial de CartSummary, clique com botão direito na pasta exibições/ShoppingCart e selecione Adicionar modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="46394-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="46394-116">Nomeie a exibição CartSummary e marque a caixa de seleção "Criar uma exibição parcial", conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="46394-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="46394-117">O modo de exibição parcial CartSummary é muito simple - é apenas um link para o modo de exibição de índice ShoppingCart que mostra o número de itens no carrinho.</span><span class="sxs-lookup"><span data-stu-id="46394-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="46394-118">O código completo para CartSummary.cshtml é da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="46394-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="46394-119">Podemos incluir uma exibição parcial em qualquer página no site, incluindo o mestre do Site, usando o método Html.RenderAction.</span><span class="sxs-lookup"><span data-stu-id="46394-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="46394-120">RenderAction exige especificar o nome da ação ("CartSummary") e o nome do controlador ("ShoppingCart") como abaixo.</span><span class="sxs-lookup"><span data-stu-id="46394-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="46394-121">Antes de adicionar isso para o site de Layout, podemos também criará o Menu de gênero para que possamos fazer todas as nossas atualizações do site ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="46394-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="46394-122">Criando a exibição parcial do Menu de gênero</span><span class="sxs-lookup"><span data-stu-id="46394-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="46394-123">Fazemos isso muito mais fácil para nossos usuários navegar por meio da loja, adicionando um Menu de gênero que lista todos os gêneros disponíveis em nosso repositório.</span><span class="sxs-lookup"><span data-stu-id="46394-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="46394-124">Vamos seguir o mesmo etapas também criam uma exibição parcial de GenreMenu e, em seguida, podemos adicionar ambos ao mestre do Site.</span><span class="sxs-lookup"><span data-stu-id="46394-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="46394-125">Primeiro, adicione a seguinte ação do controlador GenreMenu para o StoreController:</span><span class="sxs-lookup"><span data-stu-id="46394-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="46394-126">Essa ação retorna uma lista de gêneros que serão exibidos pela exibição parcial, que criaremos em seguida.</span><span class="sxs-lookup"><span data-stu-id="46394-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="46394-127">*Observação: Adicionamos o atributo [ChildActionOnly] para esta ação de controlador, que indica que apenas queremos que essa ação a ser usado em uma exibição parcial. Esse atributo impedirá que a ação do controlador que está sendo executado, navegando até /Store/GenreMenu. Isso não é necessário para exibições parciais, mas é uma boa prática, uma vez que queremos nos assegurar-se de que nossas ações do controlador são usadas como pretendemos. Estamos retornando também PartialView em vez de exibição, que permite que o mecanismo de exibição sabe que ele não deve usar o Layout para esta exibição, pois ele está sendo incluído em outros modos de exibição.*</span><span class="sxs-lookup"><span data-stu-id="46394-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="46394-128">Clique duas vezes em que a ação do controlador GenreMenu e criar uma exibição parcial chamada GenreMenu que é fortemente tipado usando a classe de dados de exibição do gênero, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="46394-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="46394-129">Atualize o código de exibição para o modo de exibição parcial GenreMenu exibir os itens usando uma lista não ordenada da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="46394-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="46394-130">Atualizando o Layout do Site para exibir nosso exibições parciais</span><span class="sxs-lookup"><span data-stu-id="46394-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="46394-131">Podemos adicionar nossa exibições parciais para o Layout do Site (/exibições/Shared/\_layout. cshtml) chamando Html.RenderAction().</span><span class="sxs-lookup"><span data-stu-id="46394-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="46394-132">Vamos adicioná-los no, bem como algumas marcações adicionais para exibi-los, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="46394-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="46394-133">Agora quando executamos o aplicativo, veremos o gênero na área de navegação à esquerda e o resumo de carrinho na parte superior.</span><span class="sxs-lookup"><span data-stu-id="46394-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="46394-134">Atualizar para a página Procurar Store</span><span class="sxs-lookup"><span data-stu-id="46394-134">Update to the Store Browse page</span></span>

<span data-ttu-id="46394-135">A página Procurar Store está funcionando, mas não parece muito bom.</span><span class="sxs-lookup"><span data-stu-id="46394-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="46394-136">Podemos atualizar a página para mostrar os álbuns em um layout melhor atualizando o código de exibição (encontrado no /Views/Store/Browse.cshtml) da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="46394-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="46394-137">Aqui estamos fazendo uso de Action em vez de HTML. ActionLink para que podemos aplicar formatação especial para o link para incluir a arte do álbum.</span><span class="sxs-lookup"><span data-stu-id="46394-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="46394-138">*Observação: Estamos estiver exibindo uma capa do álbum genérico para esses álbuns. Essas informações são armazenadas no banco de dados e pode ser editadas por meio do Gerenciador de Store. Você está bem-vindo ao adicionar sua própria arte final.*</span><span class="sxs-lookup"><span data-stu-id="46394-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="46394-139">Agora quando podemos navegar para um gênero, veremos os álbuns mostrados em uma grade com a arte do álbum.</span><span class="sxs-lookup"><span data-stu-id="46394-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="46394-140">Atualizando a Home Page para mostrar superior álbuns de venda</span><span class="sxs-lookup"><span data-stu-id="46394-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="46394-141">Queremos que nossos mais vendidos álbuns na home page para aumentar as vendas de recursos.</span><span class="sxs-lookup"><span data-stu-id="46394-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="46394-142">Faremos algumas atualizações para nossa HomeController para lidar com isso e adicionar alguns elementos de gráficos adicionais.</span><span class="sxs-lookup"><span data-stu-id="46394-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="46394-143">Primeiro, vamos adicionar uma propriedade de navegação à nossa classe de álbum para que o EntityFramework sabe que estão associadas.</span><span class="sxs-lookup"><span data-stu-id="46394-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="46394-144">As últimas linhas do nosso **álbum** classe agora deve ser assim:</span><span class="sxs-lookup"><span data-stu-id="46394-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="46394-145">*Observação: Será necessário adicionar um usando a instrução para colocar no namespace Collections.*</span><span class="sxs-lookup"><span data-stu-id="46394-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="46394-146">Primeiro, vamos adicionar um campo de storeDB e o MvcMusicStore.Models usando instruções, como em nossos outros controladores.</span><span class="sxs-lookup"><span data-stu-id="46394-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="46394-147">Em seguida, adicionaremos o seguinte método para o HomeController que consulta o nosso banco de dados para localizar as principais álbuns de vendas de acordo com a OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="46394-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="46394-148">Esse é um método privado, já que não queremos disponibilizá-lo como uma ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="46394-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="46394-149">Incluímos-lo no HomeController para manter a simplicidade, mas você é incentivado a mover sua lógica de negócios em classes de serviço separado conforme apropriado.</span><span class="sxs-lookup"><span data-stu-id="46394-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="46394-150">Com isso funcionando, podemos atualizar a ação do controlador de índice para consultar os 5 principais álbuns e retorná-los para o modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="46394-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="46394-151">O código completo para o HomeController atualizado é conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="46394-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="46394-152">Por fim, precisaremos atualizar exibição nosso índice de início para que ele possa exibir uma lista dos álbuns atualizando o tipo de modelo e adicionar a lista de álbum na parte inferior.</span><span class="sxs-lookup"><span data-stu-id="46394-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="46394-153">Faremos essa oportunidade para também adicionar um título e uma seção de promoção para a página.</span><span class="sxs-lookup"><span data-stu-id="46394-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="46394-154">Agora quando executamos o aplicativo, veremos nosso home page atualizada com principais álbuns de vendas e nossa mensagem promocional.</span><span class="sxs-lookup"><span data-stu-id="46394-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="46394-155">Conclusão</span><span class="sxs-lookup"><span data-stu-id="46394-155">Conclusion</span></span>

<span data-ttu-id="46394-156">Já vimos que ASP.NET MVC torna mais fácil para criar um site da Web sofisticado com acesso de banco de dados, associação, AJAX, etc.</span><span class="sxs-lookup"><span data-stu-id="46394-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="46394-157">muito rapidamente.</span><span class="sxs-lookup"><span data-stu-id="46394-157">pretty quickly.</span></span> <span data-ttu-id="46394-158">Espero que este tutorial tenha fornecido as ferramentas que você precisa para começar a criação de aplicativos ASP.NET MVC!</span><span class="sxs-lookup"><span data-stu-id="46394-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


> [!div class="step-by-step"]
> [<span data-ttu-id="46394-159">Anterior</span><span class="sxs-lookup"><span data-stu-id="46394-159">Previous</span></span>](mvc-music-store-part-9.md)

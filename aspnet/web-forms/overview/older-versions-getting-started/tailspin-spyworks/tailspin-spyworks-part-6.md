---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: "Parte 6: Associação do ASP.NET | Microsoft Docs"
author: JoeStagner
description: "Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 6 adiciona a associação do ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: efb0e2bed1172f42c7f1539f016fba305c47e3eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="15c3d-104">Parte 6: Associação do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="15c3d-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="15c3d-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="15c3d-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="15c3d-106">Tailspin Spyworks demonstra como extraordinariamente simples é criar aplicativos eficientes e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="15c3d-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="15c3d-107">Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, inclusive de compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="15c3d-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="15c3d-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="15c3d-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="15c3d-109">Parte 6 adiciona a associação do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="15c3d-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a><span data-ttu-id="15c3d-110">Trabalhando com a associação do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="15c3d-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="15c3d-111">Clique em segurança</span><span class="sxs-lookup"><span data-stu-id="15c3d-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="15c3d-112">Certifique-se de que estamos usando autenticação de formulários.</span><span class="sxs-lookup"><span data-stu-id="15c3d-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="15c3d-113">Use o link "Create User" para criar alguns usuários.</span><span class="sxs-lookup"><span data-stu-id="15c3d-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="15c3d-114">Quando terminar, consulte a janela Gerenciador de soluções e atualizar a exibição.</span><span class="sxs-lookup"><span data-stu-id="15c3d-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="15c3d-115">Observe que o ASPNETDB. MDF BOM foi criado.</span><span class="sxs-lookup"><span data-stu-id="15c3d-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="15c3d-116">Esse arquivo contém as tabelas para dar suporte os serviços do ASP.NET core como associação.</span><span class="sxs-lookup"><span data-stu-id="15c3d-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="15c3d-117">Agora podemos começar a implementar o processo de check-out.</span><span class="sxs-lookup"><span data-stu-id="15c3d-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="15c3d-118">Comece criando uma página de CheckOut.aspx.</span><span class="sxs-lookup"><span data-stu-id="15c3d-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="15c3d-119">A página CheckOut.aspx deve estar disponível para usuários que estão conectados para nós serão restringir o acesso ao registrado em usuários e redireciona os usuários que não estão conectados a página de logon.</span><span class="sxs-lookup"><span data-stu-id="15c3d-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="15c3d-120">Para fazer isso, adicionaremos o seguinte à seção de configuração de nosso arquivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="15c3d-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="15c3d-121">O modelo para aplicativos ASP.NET Web Forms automaticamente adicionada uma seção de autenticação para nosso arquivo Web. config e estabelecer a página de logon padrão.</span><span class="sxs-lookup"><span data-stu-id="15c3d-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="15c3d-122">Podemos deve modificar aspx arquivo code-behind para migrar um carrinho de compras anônimo quando o usuário fizer logon.</span><span class="sxs-lookup"><span data-stu-id="15c3d-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="15c3d-123">Altere a página\_evento de carregamento da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="15c3d-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="15c3d-124">Em seguida, adicione um manipulador de eventos de "Logon efetuado" como esta para definir o nome da sessão para o usuário conectado recentemente e alterar a id de sessão temporária no carrinho de compras para que o usuário chamando o método MigrateCart em nossa classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="15c3d-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="15c3d-125">(Implementado no arquivo. cs)</span><span class="sxs-lookup"><span data-stu-id="15c3d-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="15c3d-126">Implementar o método MigrateCart() assim.</span><span class="sxs-lookup"><span data-stu-id="15c3d-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="15c3d-127">Checkout.aspx, usaremos um EntityDataSource e GridView em nossa página de confirmação que fizemos em nossa página de carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="15c3d-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="15c3d-128">Observe que o nosso controle GridView Especifica um manipulador de eventos "ondatabound" denominado MyList\_RowDataBound Vamos implementar o manipulador de eventos como este.</span><span class="sxs-lookup"><span data-stu-id="15c3d-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="15c3d-129">Isso mantém método total de execução das compras carrinho conforme cada linha está associada e atualiza a linha inferior de GridView.</span><span class="sxs-lookup"><span data-stu-id="15c3d-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="15c3d-130">Neste estágio, implementamos uma apresentação "revisão" do pedido será colocado.</span><span class="sxs-lookup"><span data-stu-id="15c3d-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="15c3d-131">Vamos lidar com um cenário de carrinho vazio adicionando algumas linhas de código para nossa página\_evento de carregamento:</span><span class="sxs-lookup"><span data-stu-id="15c3d-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="15c3d-132">Quando o usuário clica no botão "Enviar", executará o código a seguir no manipulador de eventos de clique de botão Enviar.</span><span class="sxs-lookup"><span data-stu-id="15c3d-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="15c3d-133">É a parte "principal" do processo de envio da ordem a ser implementada no método SubmitOrder() de nossa classe MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="15c3d-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="15c3d-134">SubmitOrder será:</span><span class="sxs-lookup"><span data-stu-id="15c3d-134">SubmitOrder will:</span></span>

- <span data-ttu-id="15c3d-135">Coloque todos os itens de linha no carrinho de compras e usá-las para criar um novo registro de ordem e os registros de OrderDetails associados.</span><span class="sxs-lookup"><span data-stu-id="15c3d-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="15c3d-136">Calcule a data de envio.</span><span class="sxs-lookup"><span data-stu-id="15c3d-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="15c3d-137">Limpe o carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="15c3d-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="15c3d-138">Para os fins desse aplicativo de exemplo irá calcular uma data de remessa simplesmente adicionando dois dias para a data atual.</span><span class="sxs-lookup"><span data-stu-id="15c3d-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="15c3d-139">Executando o aplicativo agora também permite que nós para testar o processo de compra do início ao fim.</span><span class="sxs-lookup"><span data-stu-id="15c3d-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="15c3d-140">[Anterior](tailspin-spyworks-part-5.md)
[Próximo](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="15c3d-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>

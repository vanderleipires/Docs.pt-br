---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Usando a API da Web com Web Forms do ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/01/2017
ms.locfileid: "26536075"
---
<a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="18256-102">Usando a API da Web com Web Forms do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="18256-102">Using Web API with ASP.NET Web Forms</span></span>
====================
<span data-ttu-id="18256-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="18256-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="18256-104">Embora a API da Web do ASP.NET é empacotado com o ASP.NET MVC, é fácil adicionar API da Web a um aplicativo de Web Forms do ASP.NET tradicional.</span><span class="sxs-lookup"><span data-stu-id="18256-104">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span> <span data-ttu-id="18256-105">Este tutorial orienta você pelas etapas.</span><span class="sxs-lookup"><span data-stu-id="18256-105">This tutorial walks you through the steps.</span></span>

## <a name="overview"></a><span data-ttu-id="18256-106">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="18256-106">Overview</span></span>

<span data-ttu-id="18256-107">Para usar a API da Web em um aplicativo de Web Forms, há duas etapas principais:</span><span class="sxs-lookup"><span data-stu-id="18256-107">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="18256-108">Adicionar um controlador de API da Web que deriva de **ApiController** classe.</span><span class="sxs-lookup"><span data-stu-id="18256-108">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="18256-109">Adicionar uma tabela de rota para o **aplicativo\_iniciar** método.</span><span class="sxs-lookup"><span data-stu-id="18256-109">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="18256-110">Criar um projeto de formulários da Web</span><span class="sxs-lookup"><span data-stu-id="18256-110">Create a Web Forms Project</span></span>

<span data-ttu-id="18256-111">Inicie o Visual Studio e selecione **novo projeto** do **iniciar** página.</span><span class="sxs-lookup"><span data-stu-id="18256-111">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="18256-112">Ou, do **arquivo** menu, selecione **novo** e **projeto**.</span><span class="sxs-lookup"><span data-stu-id="18256-112">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="18256-113">No **modelos** painel, selecione **modelos instalados** e expanda o **Visual C#** nó.</span><span class="sxs-lookup"><span data-stu-id="18256-113">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="18256-114">Em **Visual C#**, selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="18256-114">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="18256-115">Na lista de modelos de projeto, selecione **aplicativo ASP.NET Web Forms**.</span><span class="sxs-lookup"><span data-stu-id="18256-115">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="18256-116">Insira um nome para o projeto e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="18256-116">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="18256-117">Criar o modelo e o controlador</span><span class="sxs-lookup"><span data-stu-id="18256-117">Create the Model and Controller</span></span>

<span data-ttu-id="18256-118">Este tutorial usa as mesmas classes de modelo e o controlador como o [Introdução](tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="18256-118">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="18256-119">Primeiro, adicione uma classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="18256-119">First, add a model class.</span></span> <span data-ttu-id="18256-120">Em **Solution Explorer**, clique com o botão direito e selecione **Adicionar classe**.</span><span class="sxs-lookup"><span data-stu-id="18256-120">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="18256-121">Nomeie a classe de produto e, em seguida, adicione a implementação a seguir:</span><span class="sxs-lookup"><span data-stu-id="18256-121">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="18256-122">Em seguida, adicione um controlador Web API para o projeto, um *controlador* é o objeto que manipula solicitações HTTP para a API da Web.</span><span class="sxs-lookup"><span data-stu-id="18256-122">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="18256-123">Em **Solution Explorer**, clique com o botão direito.</span><span class="sxs-lookup"><span data-stu-id="18256-123">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="18256-124">Selecione **Adicionar Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="18256-124">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="18256-125">Em **modelos instalados**, expanda **Visual C#** e selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="18256-125">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="18256-126">Em seguida, na lista de modelos, selecione **classe do controlador Web API**.</span><span class="sxs-lookup"><span data-stu-id="18256-126">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="18256-127">Nome do controlador "ProductsController" e clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="18256-127">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="18256-128">O **Adicionar Novo Item** assistente criará um arquivo chamado ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="18256-128">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="18256-129">Excluir os métodos que o assistente incluído e adicione os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="18256-129">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="18256-130">Para obter mais informações sobre o código neste controlador, consulte o [Introdução](tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="18256-130">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="18256-131">Adicionar informações de roteamento</span><span class="sxs-lookup"><span data-stu-id="18256-131">Add Routing Information</span></span>

<span data-ttu-id="18256-132">Em seguida, vamos adicionar uma rota URI assim que URIs do formulário &quot;/api/produtos/&quot; são roteadas para o controlador.</span><span class="sxs-lookup"><span data-stu-id="18256-132">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="18256-133">Em **Solution Explorer**, clique duas vezes em global. asax para abrir o arquivo de code-behind asax.</span><span class="sxs-lookup"><span data-stu-id="18256-133">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="18256-134">Adicione o seguinte **usando** instrução.</span><span class="sxs-lookup"><span data-stu-id="18256-134">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="18256-135">Em seguida, adicione o seguinte código para o **aplicativo\_iniciar** método:</span><span class="sxs-lookup"><span data-stu-id="18256-135">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="18256-136">Para obter mais informações sobre tabelas de roteamento, consulte [roteamento na API da Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="18256-136">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="18256-137">Adicionar cliente AJAX</span><span class="sxs-lookup"><span data-stu-id="18256-137">Add Client-Side AJAX</span></span>

<span data-ttu-id="18256-138">Isso é tudo o que você precisa criar uma API da web do que os clientes podem acessar.</span><span class="sxs-lookup"><span data-stu-id="18256-138">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="18256-139">Agora vamos adicionar uma página HTML que usa o jQuery para chamar a API.</span><span class="sxs-lookup"><span data-stu-id="18256-139">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="18256-140">Verifique se a página mestra (por exemplo, *Site.Master*) inclui um `ContentPlaceHolder` com `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="18256-140">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="18256-141">Abra o arquivo default. aspx.</span><span class="sxs-lookup"><span data-stu-id="18256-141">Open the file Default.aspx.</span></span> <span data-ttu-id="18256-142">Substitua o texto de boilerplate que está na seção de conteúdo principal, conforme mostrado:</span><span class="sxs-lookup"><span data-stu-id="18256-142">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="18256-143">Em seguida, adicione uma referência ao arquivo de origem jQuery no `HeaderContent` seção:</span><span class="sxs-lookup"><span data-stu-id="18256-143">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="18256-144">Observação: Você pode facilmente adicionar referência de script arrastando e soltando o arquivo de **Solution Explorer** na janela do editor de código.</span><span class="sxs-lookup"><span data-stu-id="18256-144">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="18256-145">Abaixo da marca de script jQuery, adicione o seguinte bloco de script:</span><span class="sxs-lookup"><span data-stu-id="18256-145">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="18256-146">Quando o documento for carregado, esse script faz uma solicitação AJAX para &quot;api/produtos&quot;.</span><span class="sxs-lookup"><span data-stu-id="18256-146">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="18256-147">A solicitação retorna uma lista de produtos no formato JSON.</span><span class="sxs-lookup"><span data-stu-id="18256-147">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="18256-148">O script adiciona as informações de produto para a tabela HTML.</span><span class="sxs-lookup"><span data-stu-id="18256-148">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="18256-149">Quando você executa o aplicativo, ele deve ser assim:</span><span class="sxs-lookup"><span data-stu-id="18256-149">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)

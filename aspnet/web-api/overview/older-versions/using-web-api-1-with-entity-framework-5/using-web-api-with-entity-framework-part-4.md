---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Parte 4: Adicionando uma exibição de administração | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: cbf42f1dbd744d7b85dde7d2dcd99a13c6208a13
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879524"
---
<a name="part-4-adding-an-admin-view"></a><span data-ttu-id="a3576-102">Parte 4: Adicionando uma exibição de administração</span><span class="sxs-lookup"><span data-stu-id="a3576-102">Part 4: Adding an Admin View</span></span>
====================
<span data-ttu-id="a3576-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a3576-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a3576-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="a3576-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a><span data-ttu-id="a3576-105">Adicionar um modo de exibição de administração</span><span class="sxs-lookup"><span data-stu-id="a3576-105">Add an Admin View</span></span>

<span data-ttu-id="a3576-106">Agora vamos ativar ao lado do cliente e adicione uma página que pode consumir dados do controlador de administrador.</span><span class="sxs-lookup"><span data-stu-id="a3576-106">Now we'll turn to the client side, and add a page that can consume data from the Admin controller.</span></span> <span data-ttu-id="a3576-107">A página permitirá aos usuários criar, editar ou excluir produtos, enviando solicitações do AJAX para o controlador.</span><span class="sxs-lookup"><span data-stu-id="a3576-107">The page will allow users to create, edit, or delete products, by sending AJAX requests to the controller.</span></span>

<span data-ttu-id="a3576-108">No Gerenciador de soluções, expanda a pasta controladores e abra o arquivo chamado HomeController.</span><span class="sxs-lookup"><span data-stu-id="a3576-108">In Solution Explorer, expand the Controllers folder and open the file named HomeController.cs.</span></span> <span data-ttu-id="a3576-109">Esse arquivo contém um controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="a3576-109">This file contains an MVC controller.</span></span> <span data-ttu-id="a3576-110">Adicionar um método chamado `Admin`:</span><span class="sxs-lookup"><span data-stu-id="a3576-110">Add a method named `Admin`:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

<span data-ttu-id="a3576-111">O **HttpRouteUrl** método cria o URI para a API da web e armazenamos isso o multiconjunto da exibição para mais tarde.</span><span class="sxs-lookup"><span data-stu-id="a3576-111">The **HttpRouteUrl** method creates the URI to the web API, and we store this in the view bag for later.</span></span>

<span data-ttu-id="a3576-112">Em seguida, posicione o cursor do texto dentro de `Admin` método de ação, em seguida, clique com botão direito e selecione **adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="a3576-112">Next, position the text cursor within the `Admin` action method, then right-click and select **Add View**.</span></span> <span data-ttu-id="a3576-113">Isso abrirá o **adicionar exibição** caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="a3576-113">This will bring up the **Add View** dialog.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

<span data-ttu-id="a3576-114">No **adicionar exibição** caixa de diálogo, o nome de exibição "Admin".</span><span class="sxs-lookup"><span data-stu-id="a3576-114">In the **Add View** dialog, name the view "Admin".</span></span> <span data-ttu-id="a3576-115">Selecione a caixa de seleção **criar uma exibição fortemente tipada**.</span><span class="sxs-lookup"><span data-stu-id="a3576-115">Select the check box labeled **Create a strongly-typed view**.</span></span> <span data-ttu-id="a3576-116">Em **modelo de classe**, selecione "Product (ProductStore.Models)".</span><span class="sxs-lookup"><span data-stu-id="a3576-116">Under **Model Class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="a3576-117">Deixe todas as outras opções como seus valores padrão.</span><span class="sxs-lookup"><span data-stu-id="a3576-117">Leave all the other options as their default values.</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

<span data-ttu-id="a3576-118">Clicando em **Add** adiciona um arquivo chamado Admin.cshtml em exibições/inicial.</span><span class="sxs-lookup"><span data-stu-id="a3576-118">Clicking **Add** adds a file named Admin.cshtml under Views/Home.</span></span> <span data-ttu-id="a3576-119">Abra o arquivo e adicionar o HTML a seguir.</span><span class="sxs-lookup"><span data-stu-id="a3576-119">Open this file and add the following HTML.</span></span> <span data-ttu-id="a3576-120">Este HTML define a estrutura da página, mas nenhuma funcionalidade está conectada ainda.</span><span class="sxs-lookup"><span data-stu-id="a3576-120">This HTML defines the structure of the page, but no functionality is wired up yet.</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a><span data-ttu-id="a3576-121">Criar um Link para a página de administração</span><span class="sxs-lookup"><span data-stu-id="a3576-121">Create a Link to the Admin Page</span></span>

<span data-ttu-id="a3576-122">No Gerenciador de soluções, expanda a pasta de modos de exibição e, em seguida, expanda a pasta compartilhada.</span><span class="sxs-lookup"><span data-stu-id="a3576-122">In Solution Explorer, expand the Views folder and then expand the Shared folder.</span></span> <span data-ttu-id="a3576-123">Abra o arquivo chamado \_cshtml.</span><span class="sxs-lookup"><span data-stu-id="a3576-123">Open the file named \_Layout.cshtml.</span></span> <span data-ttu-id="a3576-124">Localize o **ul** elemento com id = "menu" e um link de ação para a exibição do administrador:</span><span class="sxs-lookup"><span data-stu-id="a3576-124">Locate the **ul** element with id = "menu", and an action link for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> <span data-ttu-id="a3576-125">O projeto de exemplo, fiz algumas outras alterações superficiais, como substituir a cadeia de caracteres "Seu logotipo aqui".</span><span class="sxs-lookup"><span data-stu-id="a3576-125">In the sample project, I made a few other cosmetic changes, such as replacing the string "Your logo here".</span></span> <span data-ttu-id="a3576-126">Elas não afetam a funcionalidade do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a3576-126">These don't affect the functionality of the application.</span></span> <span data-ttu-id="a3576-127">Você pode baixar o projeto e comparar os arquivos.</span><span class="sxs-lookup"><span data-stu-id="a3576-127">You can download the project and compare the files.</span></span>


<span data-ttu-id="a3576-128">Execute o aplicativo e clique no link "Admin" que aparece na parte superior da home page.</span><span class="sxs-lookup"><span data-stu-id="a3576-128">Run the application and click the "Admin" link that appears at the top of the home page.</span></span> <span data-ttu-id="a3576-129">A página de administrador deve ser semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="a3576-129">The Admin page should look like the following:</span></span>

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

<span data-ttu-id="a3576-130">Direita agora, a página não faz nada.</span><span class="sxs-lookup"><span data-stu-id="a3576-130">Right now, the page doesn't do anything.</span></span> <span data-ttu-id="a3576-131">Na próxima seção, usaremos Knockout. js para criar uma interface do usuário dinâmica.</span><span class="sxs-lookup"><span data-stu-id="a3576-131">In the next section, we'll use Knockout.js to create a dynamic UI.</span></span>

## <a name="add-authorization"></a><span data-ttu-id="a3576-132">Adicione autorização</span><span class="sxs-lookup"><span data-stu-id="a3576-132">Add Authorization</span></span>

<span data-ttu-id="a3576-133">A página de administração é acessível no momento para qualquer pessoa que visite o site.</span><span class="sxs-lookup"><span data-stu-id="a3576-133">The Admin page is currently accessible to anyone visiting the site.</span></span> <span data-ttu-id="a3576-134">Vamos alterar isso para restringir a permissão para administradores.</span><span class="sxs-lookup"><span data-stu-id="a3576-134">Let's change this to restrict permission to administrators.</span></span>

<span data-ttu-id="a3576-135">Comece adicionando uma função "Administrador" e um usuário administrador.</span><span class="sxs-lookup"><span data-stu-id="a3576-135">Start by adding an "Administrator" role and an administrator user.</span></span> <span data-ttu-id="a3576-136">No Gerenciador de soluções, expanda a pasta de filtros e abra o arquivo chamado InitializeSimpleMembershipAttribute.cs.</span><span class="sxs-lookup"><span data-stu-id="a3576-136">In Solution Explorer, expand the Filters folder and open the file named InitializeSimpleMembershipAttribute.cs.</span></span> <span data-ttu-id="a3576-137">Localize o `SimpleMembershipInitializer` construtor.</span><span class="sxs-lookup"><span data-stu-id="a3576-137">Locate the `SimpleMembershipInitializer` constructor.</span></span> <span data-ttu-id="a3576-138">Após a chamada a **WebSecurity.InitializeDatabaseConnection**, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="a3576-138">After the call to **WebSecurity.InitializeDatabaseConnection**, add the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

<span data-ttu-id="a3576-139">Essa é uma maneira das para adicionar a função "Administrador" e criar um usuário para a função.</span><span class="sxs-lookup"><span data-stu-id="a3576-139">This is a quick-and-dirty way to add the "Administrator" role and create a user for the role.</span></span>

<span data-ttu-id="a3576-140">No Gerenciador de soluções, expanda a pasta controladores e abra o arquivo HomeController.</span><span class="sxs-lookup"><span data-stu-id="a3576-140">In Solution Explorer, expand the Controllers folder and open the HomeController.cs file.</span></span> <span data-ttu-id="a3576-141">Adicionar o **autorizar** de atributo para o `Admin` método.</span><span class="sxs-lookup"><span data-stu-id="a3576-141">Add the **Authorize** attribute to the `Admin` method.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

<span data-ttu-id="a3576-142">Abra o arquivo AdminController.cs e adicione o **autorizar** a todo o atributo `AdminController` classe.</span><span class="sxs-lookup"><span data-stu-id="a3576-142">Open the AdminController.cs file and add the **Authorize** attribute to the entire `AdminController` class.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> <span data-ttu-id="a3576-143">MVC e a API da Web definir **autorizar** atributos em namespaces diferentes.</span><span class="sxs-lookup"><span data-stu-id="a3576-143">MVC and Web API both define **Authorize** attributes, in different namespaces.</span></span> <span data-ttu-id="a3576-144">O MVC usa **System.Web.Mvc.AuthorizeAttribute**, enquanto a API da Web usa **System.Web.Http.AuthorizeAttribute**.</span><span class="sxs-lookup"><span data-stu-id="a3576-144">MVC uses **System.Web.Mvc.AuthorizeAttribute**, while Web API uses **System.Web.Http.AuthorizeAttribute**.</span></span>


<span data-ttu-id="a3576-145">Agora apenas administradores podem exibir a página de administração.</span><span class="sxs-lookup"><span data-stu-id="a3576-145">Now only administrators can view the Admin page.</span></span> <span data-ttu-id="a3576-146">Além disso, se você enviar uma solicitação HTTP para o controlador de administração, a solicitação deve conter um cookie de autenticação.</span><span class="sxs-lookup"><span data-stu-id="a3576-146">Also, if you send an HTTP request to the Admin controller, the request must contain an authentication cookie.</span></span> <span data-ttu-id="a3576-147">Caso contrário, o servidor envia uma resposta HTTP 401 (não autorizado).</span><span class="sxs-lookup"><span data-stu-id="a3576-147">If not, the server sends an HTTP 401 (Unauthorized) response.</span></span> <span data-ttu-id="a3576-148">Você pode ver isso na Fiddler enviando uma solicitação GET para `http://localhost:*port*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="a3576-148">You can see this in Fiddler by sending a GET request to `http://localhost:*port*/api/admin`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a3576-149">[Anterior](using-web-api-with-entity-framework-part-3.md)
> [Próximo](using-web-api-with-entity-framework-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="a3576-149">[Previous](using-web-api-with-entity-framework-part-3.md)
[Next](using-web-api-with-entity-framework-part-5.md)</span></span>

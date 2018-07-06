---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Parte 1: Visão geral e criação do projeto | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0540f3142d73fef616e30544bb1130b75c0bb436
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816299"
---
<a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="35af6-102">Parte 1: Visão geral e criação do projeto</span><span class="sxs-lookup"><span data-stu-id="35af6-102">Part 1: Overview and Creating the Project</span></span>
====================
<span data-ttu-id="35af6-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="35af6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="35af6-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="35af6-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="35af6-105">Entity Framework é uma estrutura de mapeamento relacional/objeto.</span><span class="sxs-lookup"><span data-stu-id="35af6-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="35af6-106">Ele mapeia os objetos de domínio em seu código para entidades em um banco de dados relacional.</span><span class="sxs-lookup"><span data-stu-id="35af6-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="35af6-107">Geralmente, você não precisa se preocupar sobre a camada de banco de dados, pois o Entity Framework cuida para você.</span><span class="sxs-lookup"><span data-stu-id="35af6-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="35af6-108">O código manipula os objetos e as alterações são persistidas para um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="35af6-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="35af6-109">Sobre o Tutorial</span><span class="sxs-lookup"><span data-stu-id="35af6-109">About the Tutorial</span></span>

<span data-ttu-id="35af6-110">Neste tutorial, você criará um aplicativo simples do repositório.</span><span class="sxs-lookup"><span data-stu-id="35af6-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="35af6-111">Há duas partes principais para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="35af6-111">There are two main parts to the application.</span></span> <span data-ttu-id="35af6-112">Usuários normais podem visualizar os produtos e criam pedidos:</span><span class="sxs-lookup"><span data-stu-id="35af6-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="35af6-113">Os administradores podem criar, excluir ou editar produtos:</span><span class="sxs-lookup"><span data-stu-id="35af6-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="35af6-114">Habilidades que você aprenderá</span><span class="sxs-lookup"><span data-stu-id="35af6-114">Skills You'll Learn</span></span>

<span data-ttu-id="35af6-115">Aqui está o que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="35af6-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="35af6-116">Como usar o Entity Framework com a API Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="35af6-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="35af6-117">Como usar Knockout. js para criar um cliente dinâmico da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="35af6-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="35af6-118">Como usar a autenticação de formulários com a API Web para autenticar usuários.</span><span class="sxs-lookup"><span data-stu-id="35af6-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="35af6-119">Embora este tutorial é independente, você talvez queira ler os tutoriais a seguir primeiro:</span><span class="sxs-lookup"><span data-stu-id="35af6-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="35af6-120">Sua primeira API de Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="35af6-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="35af6-121">Criar uma API da Web que suporta operações CRUD</span><span class="sxs-lookup"><span data-stu-id="35af6-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="35af6-122">Algum conhecimento dos [ASP.NET MVC](../../../../mvc/index.md) também é útil.</span><span class="sxs-lookup"><span data-stu-id="35af6-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="35af6-123">Visão geral</span><span class="sxs-lookup"><span data-stu-id="35af6-123">Overview</span></span>

<span data-ttu-id="35af6-124">Em um alto nível, aqui está a arquitetura do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="35af6-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="35af6-125">ASP.NET MVC gera as páginas HTML para o cliente.</span><span class="sxs-lookup"><span data-stu-id="35af6-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="35af6-126">API Web ASP.NET expõe operações CRUD nos dados (produtos e pedidos).</span><span class="sxs-lookup"><span data-stu-id="35af6-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="35af6-127">Entity Framework converte os modelos de c# usados pela API da Web em entidades de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="35af6-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="35af6-128">O diagrama a seguir mostra como os objetos de domínio são representados em várias camadas do aplicativo: A camada de banco de dados, o modelo de objeto e, finalmente, o formato de transmissão, que é usado para transmitir dados para o cliente por meio de HTTP.</span><span class="sxs-lookup"><span data-stu-id="35af6-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="35af6-129">Criar o projeto do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="35af6-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="35af6-130">Você pode criar o projeto do tutorial usando o Visual Web Developer Express ou a versão completa do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="35af6-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="35af6-131">Dos **inicie** , clique em **novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="35af6-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="35af6-132">No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="35af6-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="35af6-133">Em **Visual C#**, selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="35af6-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="35af6-134">Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="35af6-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="35af6-135">Nomeie o projeto "ProductStore" e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="35af6-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="35af6-136">No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione **aplicativo de Internet** e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="35af6-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="35af6-137">O modelo "Aplicativo de Internet" cria um aplicativo ASP.NET MVC que dá suporte à autenticação de formulários.</span><span class="sxs-lookup"><span data-stu-id="35af6-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="35af6-138">Se você executar o aplicativo agora, ele já tem alguns recursos:</span><span class="sxs-lookup"><span data-stu-id="35af6-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="35af6-139">Novos usuários podem registrar clicando no link "Registrar" no canto superior direito.</span><span class="sxs-lookup"><span data-stu-id="35af6-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="35af6-140">Os usuários registrados podem fazer logon, clicando no link "Entrar".</span><span class="sxs-lookup"><span data-stu-id="35af6-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="35af6-141">Informações de associação são mantidas em um banco de dados que é criado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="35af6-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="35af6-142">Para obter mais informações sobre a autenticação de formulários no ASP.NET MVC, consulte [instruções passo a passo: usando a autenticação de formulários no ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="35af6-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="35af6-143">Atualizar o arquivo CSS</span><span class="sxs-lookup"><span data-stu-id="35af6-143">Update the CSS File</span></span>

<span data-ttu-id="35af6-144">Esta etapa é superficial, mas isso tornará as páginas renderizado da seguinte forma as capturas de tela anteriores.</span><span class="sxs-lookup"><span data-stu-id="35af6-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="35af6-145">No Gerenciador de soluções, expanda a pasta de conteúdo e abra o arquivo chamado CSS.</span><span class="sxs-lookup"><span data-stu-id="35af6-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="35af6-146">Adicione os seguintes estilos CSS:</span><span class="sxs-lookup"><span data-stu-id="35af6-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [<span data-ttu-id="35af6-147">Avançar</span><span class="sxs-lookup"><span data-stu-id="35af6-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)

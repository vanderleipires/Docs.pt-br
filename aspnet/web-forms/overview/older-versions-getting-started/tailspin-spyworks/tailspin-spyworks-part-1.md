---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Parte 1: Arquivo -> Novo projeto | Microsoft Docs'
author: JoeStagner
description: "Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 1 abrange visão geral e o arquivo/novo projeto."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: bd840f9f3f5d723e6bc1bb35955a7770634e9483
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="part-1-file--new-project"></a><span data-ttu-id="0c88a-104">Parte 1: Arquivo -> Novo projeto</span><span class="sxs-lookup"><span data-stu-id="0c88a-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="0c88a-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="0c88a-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="0c88a-106">Tailspin Spyworks demonstra como extraordinariamente simples é criar aplicativos eficientes e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="0c88a-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="0c88a-107">Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, inclusive de compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="0c88a-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="0c88a-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="0c88a-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="0c88a-109">Parte 1 abrange visão geral e o arquivo/novo projeto.</span><span class="sxs-lookup"><span data-stu-id="0c88a-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a><span data-ttu-id="0c88a-110">Visão geral</span><span class="sxs-lookup"><span data-stu-id="0c88a-110">Overview</span></span>

<span data-ttu-id="0c88a-111">Este tutorial é uma introdução aos formulários da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0c88a-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="0c88a-112">Nós começarão lenta, para que a experiência de desenvolvimento de web de nível para iniciantes é okey.</span><span class="sxs-lookup"><span data-stu-id="0c88a-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="0c88a-113">O aplicativo que é criarei é um repositório online simple.</span><span class="sxs-lookup"><span data-stu-id="0c88a-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="0c88a-114">Os visitantes podem procurar produtos por categoria:</span><span class="sxs-lookup"><span data-stu-id="0c88a-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="0c88a-115">Eles podem exibir um único produto e adicioná-lo ao seu carrinho:</span><span class="sxs-lookup"><span data-stu-id="0c88a-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="0c88a-116">Eles podem revisar seu carrinho, removendo todos os itens que não desejam mais:</span><span class="sxs-lookup"><span data-stu-id="0c88a-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="0c88a-117">Continuar o check-out Avisar</span><span class="sxs-lookup"><span data-stu-id="0c88a-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="0c88a-118">Após a ordenação, eles veem uma tela de confirmação simples:</span><span class="sxs-lookup"><span data-stu-id="0c88a-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="0c88a-119">Vamos começar criando um novo projeto de formulários da Web do ASP.NET no Visual Studio 2010 e adicionaremos incrementalmente recursos para criar um aplicativo funcional completo.</span><span class="sxs-lookup"><span data-stu-id="0c88a-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="0c88a-120">Ao longo do caminho, vamos abordar o acesso de banco de dados, modos de exibição de lista e grade, páginas de atualização de dados, validação de dados, usando páginas mestras para layout de página consistente, AJAX, validação, a associação do usuário e muito mais.</span><span class="sxs-lookup"><span data-stu-id="0c88a-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="0c88a-121">Você pode acompanhá-lo passo a passo, ou você pode baixar o aplicativo concluído de [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="0c88a-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="0c88a-122">Você pode usar o Visual Studio 2010 ou o livre Visual Web Developer 2010 de [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="0c88a-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="0c88a-123">Para criar o aplicativo, você pode usar o SQL Server ou o livre SQL Server Express para hospedar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="0c88a-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a><span data-ttu-id="0c88a-124">Arquivo / novo projeto</span><span class="sxs-lookup"><span data-stu-id="0c88a-124">File / New Project</span></span>

<span data-ttu-id="0c88a-125">Vamos começar selecionando o novo projeto no menu arquivo no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0c88a-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="0c88a-126">Isso abre a caixa de diálogo Novo projeto.</span><span class="sxs-lookup"><span data-stu-id="0c88a-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="0c88a-127">Selecionaremos Visual C# modelos da Web de grupo à esquerda e, em seguida, escolha o modelo de "Aplicativo Web do ASP.NET" na coluna central.</span><span class="sxs-lookup"><span data-stu-id="0c88a-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="0c88a-128">Nomeie o projeto TailspinSpyworks e pressione o botão Okey.</span><span class="sxs-lookup"><span data-stu-id="0c88a-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="0c88a-129">Isso criará o nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="0c88a-129">This will create our project.</span></span> <span data-ttu-id="0c88a-130">Vamos analisar as pastas que são incluídas em nosso aplicativo no Gerenciador de soluções, no lado direito.</span><span class="sxs-lookup"><span data-stu-id="0c88a-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="0c88a-131">A solução vazia não é totalmente vazia – adiciona uma estrutura de pasta básica:</span><span class="sxs-lookup"><span data-stu-id="0c88a-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="0c88a-132">Observe as convenções implementadas pelo modelo de projeto padrão do ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="0c88a-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="0c88a-133">A pasta "Conta" implementa uma interface do usuário básica para ASP. Subsistema de associação do NET.</span><span class="sxs-lookup"><span data-stu-id="0c88a-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="0c88a-134">A pasta "Scripts" serve como o repositório de arquivos de JavaScript do lado cliente e os principais jQuery. js arquivos estão disponíveis por padrão.</span><span class="sxs-lookup"><span data-stu-id="0c88a-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="0c88a-135">A pasta "Estilos" é usada para organizar nossos visuais de site da web (folhas de estilo CSS)</span><span class="sxs-lookup"><span data-stu-id="0c88a-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="0c88a-136">Quando, pressione F5 para executar o nosso aplicativo e renderizar a página default.aspx vemos a seguir.</span><span class="sxs-lookup"><span data-stu-id="0c88a-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="0c88a-137">Nosso primeira aprimoramento de aplicativo será para substituir o arquivo Style.css do modelo de formulários da Web padrão com os arquivos de imagem associado que processarão o asthetics visual que queremos para nosso aplicativo Tailspin Spyworks e classes CSS.</span><span class="sxs-lookup"><span data-stu-id="0c88a-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="0c88a-138">Depois de fazer isso nossa página de default.aspx renderiza assim.</span><span class="sxs-lookup"><span data-stu-id="0c88a-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="0c88a-139">Observe os links de imagem na parte superior direita da página e os itens de menu que foram adicionados para a página mestra.</span><span class="sxs-lookup"><span data-stu-id="0c88a-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="0c88a-140">Somente os links de "Entrar" e "Conta" apontam para páginas que existem (geradas pelo modelo padrão) e o restante das páginas que vamos implementar durante a compilação de nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0c88a-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="0c88a-141">Também vamos realocar a página mestra para o diretório de estilos.</span><span class="sxs-lookup"><span data-stu-id="0c88a-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="0c88a-142">Embora essa seja uma preferência somente pode fazer coisas um pouco mais fácil se decida fazer nosso aplicativo "skinable" no futuro.</span><span class="sxs-lookup"><span data-stu-id="0c88a-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="0c88a-143">Depois de fazer isso, precisamos alterar a página mestra referências em todos os arquivos. aspx gerado pelo padrão páginas ASP.NET WebForms.</span><span class="sxs-lookup"><span data-stu-id="0c88a-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="0c88a-144">Avançar</span><span class="sxs-lookup"><span data-stu-id="0c88a-144">Next</span></span>](tailspin-spyworks-part-2.md)

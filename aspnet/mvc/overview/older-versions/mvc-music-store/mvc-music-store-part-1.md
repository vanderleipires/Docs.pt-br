---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Parte 1: Visão geral e arquivo -> Novo projeto | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 1 abrange visão geral e arquivo -> Novo projeto.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: d84a525221e40b79853be55069367134d17fb5ec
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833699"
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="bda17-104">Parte 1: Visão geral e arquivo -> Novo projeto</span><span class="sxs-lookup"><span data-stu-id="bda17-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="bda17-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="bda17-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="bda17-106">A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.</span><span class="sxs-lookup"><span data-stu-id="bda17-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="bda17-107">A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="bda17-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="bda17-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bda17-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="bda17-109">Parte 1 aborda a visão geral e o arquivo -&gt;novo projeto.</span><span class="sxs-lookup"><span data-stu-id="bda17-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="bda17-110">Visão geral</span><span class="sxs-lookup"><span data-stu-id="bda17-110">Overview</span></span>

<span data-ttu-id="bda17-111">A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e Visual Web Developer para desenvolvimento da web.</span><span class="sxs-lookup"><span data-stu-id="bda17-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="bda17-112">Podemos vai iniciar lentamente, portanto, a experiência de desenvolvimento para a web de nível iniciante é okey.</span><span class="sxs-lookup"><span data-stu-id="bda17-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="bda17-113">O aplicativo, vou desenvolver é uma loja de música simples.</span><span class="sxs-lookup"><span data-stu-id="bda17-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="bda17-114">Há três partes principais para o aplicativo: compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="bda17-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="bda17-115">Os visitantes podem procurar álbuns por gênero:</span><span class="sxs-lookup"><span data-stu-id="bda17-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="bda17-116">Eles podem exibir um único álbum e adicioná-lo ao seu carrinho:</span><span class="sxs-lookup"><span data-stu-id="bda17-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="bda17-117">Eles podem revisar seu carrinho, removendo todos os itens que não deseja mais querem:</span><span class="sxs-lookup"><span data-stu-id="bda17-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="bda17-118">Continuar a fazer check-out lhe pedirá para fazer logon ou registre-se para uma conta de usuário.</span><span class="sxs-lookup"><span data-stu-id="bda17-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="bda17-119">Depois de criar uma conta, eles podem concluir o pedido, preenchendo as informações de envio e de pagamento.</span><span class="sxs-lookup"><span data-stu-id="bda17-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="bda17-120">Para manter as coisas simples, estamos executando uma promoção incrível: tudo o que é gratuito se ele inserir o código de promoção "Gratuito"!</span><span class="sxs-lookup"><span data-stu-id="bda17-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="bda17-121">Após a ordenação, eles veem uma tela de confirmação simples:</span><span class="sxs-lookup"><span data-stu-id="bda17-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="bda17-122">Além das páginas do cliente faceing, também uma seção de administrador que mostra uma lista dos álbuns do qual os administradores podem criar, editar, criar e excluir álbuns:</span><span class="sxs-lookup"><span data-stu-id="bda17-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="bda17-123">1. Arquivo -&gt; novo projeto</span><span class="sxs-lookup"><span data-stu-id="bda17-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="bda17-124">A instalação do software</span><span class="sxs-lookup"><span data-stu-id="bda17-124">Installing the software</span></span>

<span data-ttu-id="bda17-125">Este tutorial começar criando um novo projeto ASP.NET MVC 3 usando o gratuito Visual Web Developer 2010 Express (que é gratuito) e, em seguida, vamos incrementalmente adicionar recursos para criar um aplicativo funcional completo.</span><span class="sxs-lookup"><span data-stu-id="bda17-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="bda17-126">Ao longo do caminho, vamos abordar o acesso de banco de dados, cenários de postagem de formulário, validação de dados, usando as páginas mestras para layout de página consistente, usando o AJAX para atualizações de página e validação, logon de usuário e muito mais.</span><span class="sxs-lookup"><span data-stu-id="bda17-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="bda17-127">Você pode acompanhá-lo passo a passo, ou você pode baixar o aplicativo concluído no [MVC de música de Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="bda17-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="bda17-128">Você pode usar o Visual Studio 2010 SP1 ou Visual Web Developer 2010 Express SP1 (uma versão gratuita do Visual Studio 2010) para compilar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bda17-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="bda17-129">Usaremos o SQL Server Compact (também gratuito) para hospedar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bda17-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="bda17-130">Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="bda17-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="bda17-131">[Pré-requisitos do visual Studio Web Developer Express SP1]</span><span class="sxs-lookup"><span data-stu-id="bda17-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="bda17-132">[Atualização de ferramentas do ASP.NET MVC 3]</span><span class="sxs-lookup"><span data-stu-id="bda17-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="bda17-133">[SQL Server Compact 4.0] - incluindo o suporte de tempo de execução e ferramentas</span><span class="sxs-lookup"><span data-stu-id="bda17-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="bda17-134">Criar um novo projeto ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="bda17-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="bda17-135">Vamos começar selecionando "Novo projeto" no menu arquivo no Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="bda17-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="bda17-136">Isso abre a caixa de diálogo Novo projeto.</span><span class="sxs-lookup"><span data-stu-id="bda17-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="bda17-137">Vamos selecionar Visual c# -&gt; modelos da Web de grupo à esquerda, em seguida, escolha o modelo "Aplicativo de Web do ASP.NET MVC 3" na coluna central.</span><span class="sxs-lookup"><span data-stu-id="bda17-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="bda17-138">Nomeie o projeto MvcMusicStore e pressione o botão Okey.</span><span class="sxs-lookup"><span data-stu-id="bda17-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="bda17-139">Isso exibirá uma caixa de diálogo secundária, que nos permite fazer algumas configurações específicas do MVC para nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="bda17-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="bda17-140">Selecione o seguinte:</span><span class="sxs-lookup"><span data-stu-id="bda17-140">Select the following:</span></span>

<span data-ttu-id="bda17-141">Modelo de projeto - selecione vazio</span><span class="sxs-lookup"><span data-stu-id="bda17-141">Project Template - select Empty</span></span>

<span data-ttu-id="bda17-142">Mecanismo de exibição – selecionar Razor</span><span class="sxs-lookup"><span data-stu-id="bda17-142">View Engine - select Razor</span></span>

<span data-ttu-id="bda17-143">Use marcação semântica HTML5 - check</span><span class="sxs-lookup"><span data-stu-id="bda17-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="bda17-144">Verifique se que suas configurações são mostradas conforme abaixo e, em seguida, pressione o botão Okey.</span><span class="sxs-lookup"><span data-stu-id="bda17-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="bda17-145">Isso criará o nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="bda17-145">This will create our project.</span></span> <span data-ttu-id="bda17-146">Vamos dar uma olhada em pastas que foram adicionados ao nosso aplicativo no Gerenciador de soluções, no lado direito.</span><span class="sxs-lookup"><span data-stu-id="bda17-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="bda17-147">O modelo MVC 3 vazio não está completamente vazio – ele adiciona uma estrutura de pasta básica:</span><span class="sxs-lookup"><span data-stu-id="bda17-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="bda17-148">ASP.NET MVC faz uso de algumas convenções básicas de nomenclatura para nomes de pastas:</span><span class="sxs-lookup"><span data-stu-id="bda17-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="bda17-149">**Pasta**</span><span class="sxs-lookup"><span data-stu-id="bda17-149">**Folder**</span></span> | <span data-ttu-id="bda17-150">**Finalidade**</span><span class="sxs-lookup"><span data-stu-id="bda17-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="bda17-151">**/ Controladores**</span><span class="sxs-lookup"><span data-stu-id="bda17-151">**/Controllers**</span></span> | <span data-ttu-id="bda17-152">Controladores de respondem à entrada do navegador, decidir o que fazer com ele e retornar a resposta para o usuário.</span><span class="sxs-lookup"><span data-stu-id="bda17-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="bda17-153">**/ Modos de exibição**</span><span class="sxs-lookup"><span data-stu-id="bda17-153">**/Views**</span></span> | <span data-ttu-id="bda17-154">Modos de exibição manter nossos modelos de interface do usuário</span><span class="sxs-lookup"><span data-stu-id="bda17-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="bda17-155">**Ou os modelos**</span><span class="sxs-lookup"><span data-stu-id="bda17-155">**/Models**</span></span> | <span data-ttu-id="bda17-156">Modelos de mantenham e manipulam dados</span><span class="sxs-lookup"><span data-stu-id="bda17-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="bda17-157">**/Content**</span><span class="sxs-lookup"><span data-stu-id="bda17-157">**/Content**</span></span> | <span data-ttu-id="bda17-158">Essa pasta contém nossas imagens, CSS e qualquer outro conteúdo estático</span><span class="sxs-lookup"><span data-stu-id="bda17-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="bda17-159">**/ Scripts**</span><span class="sxs-lookup"><span data-stu-id="bda17-159">**/Scripts**</span></span> | <span data-ttu-id="bda17-160">Essa pasta contém nossos arquivos de JavaScript</span><span class="sxs-lookup"><span data-stu-id="bda17-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="bda17-161">Essas pastas são incluídas, mesmo em um aplicativo ASP.NET MVC vazio porque a estrutura ASP.NET MVC, por padrão, usa uma abordagem de "convention over configuration" e faz algumas suposições padrão com base nas convenções de nomenclatura de pasta.</span><span class="sxs-lookup"><span data-stu-id="bda17-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="bda17-162">Por exemplo, controladores procurarem exibições na pasta modos de exibição por padrão sem a necessidade de especificar isso explicitamente em seu código.</span><span class="sxs-lookup"><span data-stu-id="bda17-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="bda17-163">Adequar-se com as convenções padrão reduz a quantidade de código que você precisa para escrever, e pode também torna mais fácil para outros desenvolvedores a entender o seu projeto.</span><span class="sxs-lookup"><span data-stu-id="bda17-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="bda17-164">Vamos explicar essas convenções mais durante a compilação de nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bda17-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bda17-165">Avançar</span><span class="sxs-lookup"><span data-stu-id="bda17-165">Next</span></span>](mvc-music-store-part-2.md)

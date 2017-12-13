---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: "Parte 1: Visão geral e arquivo -> Novo projeto | Microsoft Docs"
author: jongalloway
description: "Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 1 abrange visão geral e o arquivo -> Novo projeto."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 1e3373a21c7d1766cfad390a7ba68b1363d8d895
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="173e5-104">Parte 1: Visão geral e arquivo -> Novo projeto</span><span class="sxs-lookup"><span data-stu-id="173e5-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="173e5-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="173e5-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="173e5-106">O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.</span><span class="sxs-lookup"><span data-stu-id="173e5-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="173e5-107">O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="173e5-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="173e5-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="173e5-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="173e5-109">Parte 1 abrange visão geral e o arquivo -&gt;novo projeto.</span><span class="sxs-lookup"><span data-stu-id="173e5-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="173e5-110">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="173e5-110">Overview</span></span>

<span data-ttu-id="173e5-111">O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Web Developer para desenvolvimento na web.</span><span class="sxs-lookup"><span data-stu-id="173e5-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="173e5-112">Nós começarão lenta, para que a experiência de desenvolvimento de web de nível para iniciantes é okey.</span><span class="sxs-lookup"><span data-stu-id="173e5-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="173e5-113">O aplicativo que é criarei é um repositório de música simples.</span><span class="sxs-lookup"><span data-stu-id="173e5-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="173e5-114">Há três partes principais para o aplicativo: compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="173e5-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="173e5-115">Os visitantes podem procurar álbuns por gênero:</span><span class="sxs-lookup"><span data-stu-id="173e5-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="173e5-116">Eles podem exibir um único álbum e adicioná-lo ao seu carrinho:</span><span class="sxs-lookup"><span data-stu-id="173e5-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="173e5-117">Eles podem revisar seu carrinho, removendo todos os itens que não desejam mais:</span><span class="sxs-lookup"><span data-stu-id="173e5-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="173e5-118">Continuar o check-out solicitará-los para fazer logon ou registre-se para uma conta de usuário.</span><span class="sxs-lookup"><span data-stu-id="173e5-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="173e5-119">Depois de criar uma conta, eles podem concluir a ordem de preencher as informações de pagamento e envio.</span><span class="sxs-lookup"><span data-stu-id="173e5-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="173e5-120">Para manter as coisas simples, estamos executando uma promoção incrível: tudo é grátis que insira o código de promoção "Livre"!</span><span class="sxs-lookup"><span data-stu-id="173e5-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="173e5-121">Após a ordenação, eles veem uma tela de confirmação simples:</span><span class="sxs-lookup"><span data-stu-id="173e5-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="173e5-122">Além das páginas de faceing de cliente, vamos também criar uma seção de administrador que mostra uma lista de álbuns do qual os administradores podem criar, editar e excluir álbuns:</span><span class="sxs-lookup"><span data-stu-id="173e5-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="173e5-123">1. Arquivo -&gt; novo projeto</span><span class="sxs-lookup"><span data-stu-id="173e5-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="173e5-124">Instalar o software</span><span class="sxs-lookup"><span data-stu-id="173e5-124">Installing the software</span></span>

<span data-ttu-id="173e5-125">Este tutorial começar criando um novo projeto ASP.NET MVC 3 usando o livre Visual Web Developer 2010 Express (que é gratuito) e, em seguida, vamos incrementalmente adicionar recursos para criar um aplicativo funcional completo.</span><span class="sxs-lookup"><span data-stu-id="173e5-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="173e5-126">Ao longo do caminho, vamos abordar o acesso de banco de dados, cenários de postagem de formulário, validação de dados, usando páginas mestras para layout de página consistente usando AJAX para atualizações de página e validação, logon de usuário e muito mais.</span><span class="sxs-lookup"><span data-stu-id="173e5-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="173e5-127">Você pode acompanhá-lo passo a passo, ou você pode baixar o aplicativo concluído de [repositório de música MVC](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="173e5-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="173e5-128">Você pode usar o Visual Studio 2010 SP1 ou Visual Web Developer 2010 Express SP1 (uma versão gratuita do Visual Studio 2010) para compilar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="173e5-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="173e5-129">Usaremos o SQL Server Compact (também gratuito) para hospedar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="173e5-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="173e5-130">Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="173e5-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="173e5-131">[Pré-requisitos do visual Studio Web Developer Express SP1]</span><span class="sxs-lookup"><span data-stu-id="173e5-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="173e5-132">[Atualização de ferramentas do ASP.NET MVC 3]</span><span class="sxs-lookup"><span data-stu-id="173e5-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="173e5-133">[SQL Server Compact 4.0] - incluindo o tempo de execução e ferramentas de suporte</span><span class="sxs-lookup"><span data-stu-id="173e5-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="173e5-134">Criar um novo projeto ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="173e5-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="173e5-135">Vamos começar selecionando "Novo projeto" no menu arquivo no Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="173e5-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="173e5-136">Isso abre a caixa de diálogo Novo projeto.</span><span class="sxs-lookup"><span data-stu-id="173e5-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="173e5-137">Selecionaremos Visual c# -&gt; modelos da Web grupo à esquerda, em seguida, escolha o modelo de "Aplicativo de Web do ASP.NET MVC 3" no centro da coluna.</span><span class="sxs-lookup"><span data-stu-id="173e5-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="173e5-138">Nomeie o projeto MvcMusicStore e pressione o botão Okey.</span><span class="sxs-lookup"><span data-stu-id="173e5-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="173e5-139">Isso exibirá uma caixa de diálogo secundária, que permite fazer algumas configurações específicas do MVC para nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="173e5-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="173e5-140">Selecione o seguinte:</span><span class="sxs-lookup"><span data-stu-id="173e5-140">Select the following:</span></span>

<span data-ttu-id="173e5-141">Modelo de projeto - selecione vazio</span><span class="sxs-lookup"><span data-stu-id="173e5-141">Project Template - select Empty</span></span>

<span data-ttu-id="173e5-142">Mecanismo de exibição - selecione Razor</span><span class="sxs-lookup"><span data-stu-id="173e5-142">View Engine - select Razor</span></span>

<span data-ttu-id="173e5-143">Usar marcação semântica do HTML5 - marcada</span><span class="sxs-lookup"><span data-stu-id="173e5-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="173e5-144">Verifique se as configurações são mostradas conforme abaixo e pressione o botão Okey.</span><span class="sxs-lookup"><span data-stu-id="173e5-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="173e5-145">Isso criará o nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="173e5-145">This will create our project.</span></span> <span data-ttu-id="173e5-146">Vamos analisar as pastas que foram adicionados ao nosso aplicativo no Gerenciador de soluções, no lado direito.</span><span class="sxs-lookup"><span data-stu-id="173e5-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="173e5-147">O modelo MVC 3 vazio não está totalmente vazio – adiciona uma estrutura de pasta básica:</span><span class="sxs-lookup"><span data-stu-id="173e5-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="173e5-148">ASP.NET MVC usa algumas convenções básicas de nomenclatura para nomes de pasta:</span><span class="sxs-lookup"><span data-stu-id="173e5-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="173e5-149">**Pasta**</span><span class="sxs-lookup"><span data-stu-id="173e5-149">**Folder**</span></span> | <span data-ttu-id="173e5-150">**Finalidade**</span><span class="sxs-lookup"><span data-stu-id="173e5-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="173e5-151">**/ Controladores**</span><span class="sxs-lookup"><span data-stu-id="173e5-151">**/Controllers**</span></span> | <span data-ttu-id="173e5-152">Controladores de respondem à entrada do navegador, decidir o que fazer com ele e retornar a resposta para o usuário.</span><span class="sxs-lookup"><span data-stu-id="173e5-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="173e5-153">**/ Modos de exibição**</span><span class="sxs-lookup"><span data-stu-id="173e5-153">**/Views**</span></span> | <span data-ttu-id="173e5-154">Exibições de manter os nossos modelos de interface do usuário</span><span class="sxs-lookup"><span data-stu-id="173e5-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="173e5-155">**Ou os modelos**</span><span class="sxs-lookup"><span data-stu-id="173e5-155">**/Models**</span></span> | <span data-ttu-id="173e5-156">Modelos de manter e manipulam dados</span><span class="sxs-lookup"><span data-stu-id="173e5-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="173e5-157">**/ Conteúdo**</span><span class="sxs-lookup"><span data-stu-id="173e5-157">**/Content**</span></span> | <span data-ttu-id="173e5-158">Esta pasta contém nosso imagens, CSS e qualquer outro conteúdo estático</span><span class="sxs-lookup"><span data-stu-id="173e5-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="173e5-159">**/ Scripts**</span><span class="sxs-lookup"><span data-stu-id="173e5-159">**/Scripts**</span></span> | <span data-ttu-id="173e5-160">Esta pasta contém os arquivos do JavaScript</span><span class="sxs-lookup"><span data-stu-id="173e5-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="173e5-161">Essas pastas estão incluídas mesmo em um aplicativo ASP.NET MVC vazio porque a estrutura ASP.NET MVC, por padrão, usa uma abordagem de "convenção sobre a configuração" e faz algumas suposições padrão com base em convenções de nomenclatura de pasta.</span><span class="sxs-lookup"><span data-stu-id="173e5-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="173e5-162">Por exemplo, controladores procurarem exibições na pasta exibições por padrão sem precisar especificar isso explicitamente em seu código.</span><span class="sxs-lookup"><span data-stu-id="173e5-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="173e5-163">Adequar-se com as convenções padrão reduz a quantidade de código que você precisa gravar, e também podem tornar mais fácil para outros desenvolvedores entender seu projeto.</span><span class="sxs-lookup"><span data-stu-id="173e5-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="173e5-164">Vamos explicar essas convenções mais durante a compilação de nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="173e5-164">We'll explain these conventions more as we build our application.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="173e5-165">Avançar</span><span class="sxs-lookup"><span data-stu-id="173e5-165">Next</span></span>](mvc-music-store-part-2.md)
